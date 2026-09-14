# AGENTS.md — Base de Conocimiento

RAG interno 100% local (documentos, código, Teams, work items) con citas verificables.
Sigue la convención [agents.md](https://agents.md): solo lo no obvio; lee los enlaces antes de un cambio estructural.

## Dónde encontrar las cosas

- [`docs/architecture.md`](docs/architecture.md) — pipeline de 7 etapas, contenedores, módulos, retrieval de 4 señales, autenticación.
- [`docs/java.md`](docs/java.md) — JDK, BOMs, DI, fronteras de Spring Modulith, persistencia, pruebas, estilo, gates de calidad.
- [`docs/infrastructure.md`](docs/infrastructure.md) — Docker Compose, perfiles de modelo, CI/CD y Ruleset, hooks del agente y MCP.
- [`docs/data-model.md`](docs/data-model.md) · [`docs/business.md`](docs/business.md) — esquema de datos y qué es el producto.
- [`docs/design-tokens.md`](docs/design-tokens.md) y [`COMPONENTS.md`](COMPONENTS.md) — lee `COMPONENTS.md` antes de escribir UI.
- [`docs/adrs/`](docs/adrs) — 13 decisiones, desde la tabla única de embeddings hasta el módulo de acciones independiente del RAG.
- [`docs/plans/`](docs/plans) · [`docs/investigacion-vram-y-modelo-llm.md`](docs/investigacion-vram-y-modelo-llm.md) · [`docs/teams/registro-azure-bot.md`](docs/teams/registro-azure-bot.md).
- [`docs/claims-ledger.md`](docs/claims-ledger.md) — qué afirma cada doc, su fuente y si sigue vigente.

## Comandos

```bash
cp .env.example .env && make up      # levanta db, ollama, docling-serve, api
make pull-models                     # una sola vez: embeddings + reranker
make hooks                           # una sola vez: instala los git hooks (Lefthook)
make check                           # lint + build + test: la señal local de "el repo está bien"
make gpu-check                       # qué tarjeta ve y cómo la reparte (y cómo forzarlo)
```

`make help` lista el resto (perfiles de modelo, `seed`/`ingest`, `psql`, `format`/`lint`/`secrets`/`ci`,
`jdk-check`, `docling-reciclar`). Prefiere el `Makefile` y `./mvnw` sobre invocaciones sueltas de Docker/Maven.

## Reglas no obvias

- **Los adaptadores son piel**: `web`, `teams` y `seguridad` solo cruzan por las fachadas
  `orquestacion.Consultar` (el RAG) y `acciones.Acciones`, y `compartido`. `ArquitecturaTest` rompe
  el build si llegan a `recuperacion`, `ingesta`, `modelos` o `llm`, si el núcleo depende de ellos, o
  si `web`/`teams` y `seguridad` se mezclan.
- **`acciones` es independiente del RAG**: comparte solo el vault indexado y `llm`; nunca
  `Consultar`, el planner, el retrieval ni `query_log`
  ([ADR-0013](docs/adrs/0013-modulo-acciones-independiente-del-rag.md)).
- **El `Makefile` fija su propio `SHELL` en Windows** (el `sh.exe` de Git for Windows): sin eso,
  `make` desde PowerShell cae a `cmd.exe`. Por lo mismo las recetas usan `sh ./mvnw`, no `./mvnw`.
- **La GPU se reparte según el hardware**: `make up` lee VRAM, Compute Capability y driver con
  `nvidia-smi` (LLM siempre, embeddings desde 6 GB, docling desde 8 GB); `make gpu-check` lo explica.
- **Requiere JDK 25**: si `./mvnw -v` reporta uno menor, apunta `JAVA_HOME` a un JDK 25 antes de compilar.
- **Versiones gestionadas por BOM** (`spring-modulith-bom`, `spring-ai-bom`, `testcontainers-bom`,
  `arconia-bom`): no le agregues `<version>` propia a una dependencia ya cubierta.
- **Spring AI solo como cliente**: sin `VectorStore` ni abstracciones de RAG; el retrieval de 4
  señales va a mano en SQL sobre `JdbcClient`.
- **Sigue las convenciones del código**: constructor injection, records para `*Propiedades` y `log`
  en minúscula para el logger SLF4J (lo permite `checkstyle.xml`: no lo prohíbas).
- **`jqwik.version` está fijado en 1.9.3 a propósito**: 1.10.x imprime una inyección de prompt contra
  agentes en cada corrida ([LWN.net](https://lwn.net/Articles/1075317/)). No lo subas sin revisar esa release.
- **El merge a `dev` es squash con el mensaje por defecto**, no el cuerpo de la PR: si el `Closes #N`
  vivía solo ahí, el issue no se cierra solo; ciérralo a mano.

## Pruebas

`./mvnw test` corre todo en Surefire, incluido `ArquitecturaTest`: [`docs/java.md`](docs/java.md#build-run-test).

## Estilo de código

Spotless (`google-java-format`), Checkstyle y `-Werror`, los tres bloquean: [`docs/java.md`](docs/java.md#quality-gates).

## CI

`make ci` en cada push y PR, exigido por el Ruleset de `dev` (`LEFTHOOK=0` salta los hooks locales, no el Ruleset): [`docs/infrastructure.md`](docs/infrastructure.md#cicd).

## Seguridad

- No commitees `.env` ni credenciales; `.env.example` es la lista canónica de variables. No registres
  secretos, tokens ni datos personales en logs: todo en este repo es legible por un agente.
- `gitleaks` escanea en pre-commit y CI (`.gitleaks.toml`). Un falso positivo verificado se silencia
  por su *fingerprint* exacto en `.gitleaksignore`, nunca por ruta de archivo.

## Hooks del agente

6 scripts en `scripts/agent-hooks/`, solo para Claude Code: [`docs/infrastructure.md`](docs/infrastructure.md#hooks).

## MCP (Model Context Protocol)

GitHub y DBHub (lectura y escritura sobre la base real): [`docs/infrastructure.md`](docs/infrastructure.md#mcp).
