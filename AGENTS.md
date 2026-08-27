# AGENTS.md — Base de Conocimiento

RAG interno 100% local (documentos, código, Teams, work items) con citas verificables.
Sigue la convención [agents.md](https://agents.md).

Este archivo solo captura lo que no es obvio leyendo el código. Para arquitectura, modelo
de datos, decisiones y contexto más amplio, sigue los enlaces y lee la fuente.

## Dónde encontrar las cosas

- [`docs/architecture.md`](docs/architecture.md) — pipeline de 7 etapas, contenedores,
  módulos, esquema, retrieval de 4 señales, autenticación.
- [`docs/java.md`](docs/java.md) — contexto Java profundo: JDK, BOMs, DI, fronteras de
  Spring Modulith, persistencia, build/test, gates de calidad.
- [`docs/data-model.md`](docs/data-model.md) — esquema de datos completo y migraciones.
- [`docs/infrastructure.md`](docs/infrastructure.md) — Docker Compose, perfiles de
  modelo, despliegue.
- [`docs/business.md`](docs/business.md) — qué es el producto.
- [`docs/adrs/`](docs/adrs) — 11 decisiones de diseño no obvias leyendo el código.
- [`docs/plans/plan-base-conocimiento.md`](docs/plans/plan-base-conocimiento.md) — plan
  de ejecución fase por fase, con hallazgos reales.
- [`docs/investigacion-vram-y-modelo-llm.md`](docs/investigacion-vram-y-modelo-llm.md) —
  investigación de perfiles de modelo/VRAM.
- [`docs/teams/registro-azure-bot.md`](docs/teams/registro-azure-bot.md) — conectar el
  bot de Teams a un canal real.

Lee estos docs antes de hacer cambios estructurales.

## Comandos

```bash
cp .env.example .env && make up      # levanta db, ollama, docling-serve, api
make pull-models                     # una sola vez: embeddings + reranker
make hooks                           # una sola vez: instala los git hooks (Lefthook)
./mvnw test                          # pruebas, incluidos los gates de arquitectura
./mvnw verify                        # build completo
make build                           # jar sin pruebas (./mvnw -B clean package -DskipTests)
make check                           # lint + build + test: la señal local de "el repo está bien"
```

`make help` lista el resto (perfiles de modelo, `make seed`/`ingest`, `make psql`,
`make format`/`lint`/`secrets`/`ci`). Prefiere el `Makefile` y `./mvnw` sobre invocaciones
sueltas de Docker/Maven.

Requiere JDK 25 (`<java.version>` en `pom.xml`) — si `./mvnw -v` reporta un JDK menor, apunta
`JAVA_HOME` a un JDK 25 instalado antes de compilar; el runtime target no baja aunque el JDK
por defecto de la máquina sea otro.

## Reglas no obvias

- **Los adaptadores son piel**: `web` y `teams` solo pueden cruzar por la fachada
  `orquestacion.Consultar` — `ArquitecturaTest` (ArchUnit) rompe el build si alguno
  llega directo a `recuperacion`, `ingesta`, `modelos`, `llm` o `seguridad`.
- **Versiones gestionadas por BOM**: 4 BOMs (`spring-modulith-bom`, `spring-ai-bom`,
  `testcontainers-bom`, `arconia-bom`) fijan versión — no le agregues `<version>` propia
  a una dependencia ya cubierta.
- **Spring AI solo como cliente**: no se usa `VectorStore` ni las abstracciones de RAG —
  el retrieval de 4 señales va escrito a mano en SQL sobre `JdbcClient`.
- **`.env` nunca se commitea**; `.env.example` es la lista canónica de variables.

## Pruebas

`./mvnw test` corre todo bajo JUnit 5 (Surefire, sin separación unit/integration):
incluye `ArquitecturaTest` (ArchUnit + `ApplicationModules.verify()`) y pruebas con
Testcontainers (Postgres real). Mockito para dobles, AssertJ para aserciones, jqwik para
property-based, WireMock para Bot Framework/Graph/Azure DevOps. Detalle en `docs/java.md`.

## Estilo de código

`google-java-format` vía Spotless (`make format` aplica, `make lint` verifica) más Checkstyle
(`checkstyle.xml`, imports/naming/tamaño — no reformatea, solo falla) para lo que Spotless no
cubre. `-Werror` en `maven-compiler-plugin`: cualquier warning del compilador rompe el build.
Sin SpotBugs ni PMD. Sigue las convenciones ya presentes en el código (constructor injection,
records para `*Propiedades`, paquetes `internal` implícitos por Modulith, `log` en minúscula
para el logger SLF4J — así lo permite `checkstyle.xml`, no lo prohíbas).

## CI y hooks locales

`.github/workflows/ci.yml` corre `make ci` (lint + build + test + secrets) en cada push y pull
request; JDK 25 vía `actions/setup-java`, mismo que `pom.xml`. Localmente, `make hooks` instala
Lefthook: pre-commit verifica estilo y escanea secretos en lo staged, pre-push corre
`make check`. Escape hatch: `LEFTHOOK=0 git commit …`.

## Seguridad

- No commitees `.env` ni archivos con credenciales. Agrega variables nuevas a `.env.example`.
- No registres secretos, tokens ni información personal en logs.
- Asume que cualquier cosa en este repo es legible por un agente de IA — nunca pegues
  secretos aquí.
- `gitleaks` escanea en pre-commit y CI (`.gitleaks.toml`). Un falso positivo verificado se
  silencia por su *fingerprint* exacto en `.gitleaksignore` — nunca por ruta de archivo.
