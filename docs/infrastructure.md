# Infraestructura — Base de Conocimiento

## Desarrollo local

### Prerrequisitos

- Docker + Docker Compose (el `Makefile` orquesta todo sobre `compose.yml` y sus overrides).
- JDK 25 (el wrapper `./mvnw` viene commiteado, no hace falta Maven instalado). Con un JDK anterior
  el build falla con `release version 25 not supported`; `make jdk-check` lo dice antes de compilar.
- Opcional: GPU NVIDIA (`nvidia-smi`). `make up` no solo la detecta: lee VRAM, Compute Capability y
  versión del driver, y reparte en consecuencia — LLM siempre en la tarjeta, embeddings desde 6 GB,
  docling desde 8 GB. `make gpu-check` explica qué decidió y por qué; `KB_GPU`, `KB_DOCLING_GPU` y
  los dos umbrales lo fuerzan. Detalle en el [README](../README.md#reparto-de-la-gpu).
- En Windows, `make` necesita Git for Windows instalado: el `Makefile` usa su `sh.exe` como shell
  porque las recetas son POSIX. Funciona igual desde PowerShell y desde Git Bash.

El LLM es intercambiable por perfil (`make up-bonsai`, `up-ministral`, `up-qwen35`, …): son
overrides de compose encadenados sobre `compose.yml`. La tabla completa de los siete perfiles, con
su descarga y su modelo, está en el [README](../README.md#perfiles-de-modelo).

### Inicio rápido

```bash
cp .env.example .env   # completar variables, ver abajo
make pull-models       # descarga LLM, embeddings y reranker a KB_DATA_DIR (~5.5 GB, una vez)
make up                # levanta db, ollama, docling-serve y api
make health            # confirma que los 4 servicios responden
make ingest            # ingiere el corpus de ejemplo (vault/documentos)
```

### Servicios (local)

| Servicio | Imagen | Propósito |
|---|---|---|
| `db` | `pgvector/pgvector:pg18-trixie` | Postgres 18 + pgvector: tabla única de embeddings, FTS, cola, auditoría |
| `ollama` | `ollama/ollama` | `gemma3:4b` (planner/destilación/síntesis) y `bge-m3` (embeddings) |
| `docling-serve` | `quay.io/docling-project/docling-serve-cpu` | Extrae PDF/DOCX/PPTX a Markdown |
| `api` | build propio (Java, jar por capas) | Ingesta, retrieval, orquestación, UI estática, endpoint de Teams |

### Variables de entorno

- `.env.example` es la lista canónica (31 variables): puertos, credenciales de Postgres, modelo
  LLM/embeddings activos, flags de las fuentes opcionales (`KB_TEAMS_HABILITADO`,
  `KB_GRAPH_HABILITADO`, `KB_AZDO_HABILITADO`) y sus credenciales asociadas.
- Nunca commitees `.env`.

## Producción

### Objetivo de despliegue

Docker Compose en una VM/máquina propia — `make up` con el override de GPU si el host la tiene
(`compose.gpu.yml`); no hay manifiestos de Kubernetes en el repo.

### Topología

<!-- TODO: describir qué máquina/VM concreta corre esto hoy y si hay algo delante (reverse proxy,
TLS terminator). No está en el repo — es conocimiento operativo del equipo. -->

### CI/CD

- **Herramienta:** GitHub Actions, `.github/workflows/ci.yml`.
- **Trigger:** push a cualquier rama, más las PR abiertas desde forks. Un segundo push a la misma
  rama cancela la corrida anterior.
- **Pasos (job `check`):** instala gitleaks 8.30.1 (con verificación de checksum) y JDK 25, corre
  `make ci` (lint, build, pruebas y escaneo de secretos) y publica los reportes de Surefire como
  artefacto.
- **CD:** no hay. El camino a producción sigue siendo **manual**: `make up` a mano cuando hace falta.
- **Gobernanza:** un [Ruleset de GitHub](https://github.com/G3A/base-conocimiento-sandbox/rules) sobre
  `dev` exige el workflow `CI` en verde y al menos 1 aprobación antes de habilitar el merge. El
  gate local (hooks, `make check`) es una convención; el Ruleset es lo que lo vuelve obligatorio.
  Como el repo tiene una sola persona y nadie aprueba su propia PR, el Ruleset lleva el rol admin
  como bypass.

## Agente de IA (hooks y MCP)

Exclusivo de Claude Code: ningún otro agente de IA lee hoy estos archivos.

### Hooks

Instalados por `/sdlc-ia:instrument-agent-java` en `scripts/agent-hooks/` (bash puro) y
registrados en `.claude/settings.json`: 6 scripts.

| Hook | Bloquea | Qué hace |
|---|---|---|
| Secret read-guard | Sí | Antes de `Bash` y `Read`, deniega leer `.env`, claves privadas, `secrets.json`, etc. No cubre `@`-referencias ni Grep/Glob. |
| Bloqueo de comandos peligrosos | Sí | Antes de `Bash`: `rm -rf` fuera del repo, `sudo`, force-push a `main`/`dev`, `git reset --hard`, `mvn deploy`. No es un sandbox: texto, no un parser de shell. |
| Dependency sweep | No | Al iniciar o reanudar sesión, `mvn versions:display-dependency-updates`. |
| Audit log | No | Registra el `tool_input` completo de cada llamada en `logs/audit.log` (gitignored). Puede contener cualquier cosa que haya pasado por una herramienta. |
| Version-pin guard | Avisa | Tras editar `pom.xml`, avisa si una dependencia nueva trae `<version>` literal en vez de heredarla de `<dependencyManagement>`. |
| Generated-files guard | Sí | Deniega editar una migración de Flyway ya existente bajo `db/migration/`; crear la siguiente sigue permitido. |

### MCP

`.mcp.json` está committeado. Cada máquina lo aprueba una vez: correr `claude` en el repo, aceptar
el diálogo de confianza del workspace y confirmar cada servidor con `/mcp`.

| Servidor | Da acceso a | Variable de entorno |
|---|---|---|
| GitHub (HTTP, `Authorization: Bearer ${GITHUB_PAT}`) | Issues, Pull Requests, runs de Actions | `GITHUB_PAT` |
| DBHub (stdio, `npx @bytebase/dbhub@1.2.1 --dsn ${APP_DSN}`) | Lectura **y escritura** sobre la base Postgres real: DBHub ya no soporta `--readonly` | `APP_DSN` (ej. `postgres://kb:kb@localhost:5432/baseconocimiento?sslmode=disable`) |

Las dos variables se exportan en el entorno de quien use el agente; nunca se escriben literales en
el archivo. Context7 no se instaló; se puede sumar con `/sdlc-ia:instrument-agent-java`.

## Observabilidad

`spring-boot-starter-actuator` está en el classpath y sus endpoints están expuestos
(`management.endpoints.web.exposure.include: health,info,metrics`, `application.yml:101-108`), sin
acotar por perfil — el mismo `application.yml` corre en local y en producción. Nada los consume
todavía: no hay Prometheus, Grafana ni Micrometer configurado en el repo. Logs: `make logs` sigue
el log del contenedor `api`; no hay agregador centralizado configurado.

## Docs relacionados

- [Arquitectura](./architecture.md)
- [Decisiones](./adrs/)
