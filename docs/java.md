# Java — Base de Conocimiento

Contexto profundo de Java para agentes de IA. Para el pipeline funcional completo ver
[`architecture.md`](architecture.md); para el porqué de una decisión puntual,
[`adrs/`](adrs).

## Postura del framework

Spring Boot 4.1 (`spring-boot-starter-parent:4.1.0`), con Spring Modulith 2.1 encima para
las fronteras entre módulos. Servidor embebido (Tomcat vía `spring-boot-starter-web`), sin
app server externo. Módulo Maven único — no hay `<modules>` ni multi-módulo.

## JDK objetivo

`pom.xml:24` fija `<java.version>25</java.version>`, alineado con el `Dockerfile`, que
compila y corre las 3 etapas (`deps`, `build`, `runtime`) sobre
`eclipse-temurin:25-jdk-noble` / `-jre-noble`. Corregido: hasta esta revisión `pom.xml`
había quedado en 21 mientras el Dockerfile ya corría en 25.

## Gestión de dependencias y BOMs

`dependencyManagement` importa 4 BOMs además del parent de Spring Boot:
`spring-modulith-bom` 2.1.0, `spring-ai-bom` 2.0.0, `testcontainers-bom` 2.0.5,
`arconia-bom` 0.29.0. Versiones fijadas en `<properties>`, sin rangos.

**Regla para agentes**: una dependencia cubierta por uno de estos BOMs o por
`dependencyManagement` **no debe llevar su propio `<version>`** en `<dependencies>` — es
uno de los errores más comunes al agregar una dependencia nueva.

El wrapper `./mvnw` está commiteado — úsalo siempre en vez de un `mvn` de sistema.

Dependencias destacadas por área:

| Área | Dependencias |
|---|---|
| Web | `spring-boot-starter-web`, `-validation`, `-actuator` |
| Persistencia | `spring-boot-starter-jdbc` + `postgresql` + `pgvector` + Flyway (`spring-boot-flyway` + `flyway-core` + `flyway-database-postgresql`) |
| Modulith | `spring-modulith-starter-core` |
| IA | `spring-ai-starter-model-ollama` + `spring-ai-starter-model-openai` (dos proveedores a la vez, ver "Dónde termina Spring AI" en `architecture.md`) |
| Reranking local | `onnxruntime` + `ai.djl.huggingface:tokenizers` |
| Ingesta | `arconia-docling-spring-boot-starter`, `org.eclipse.jgit` |
| Seguridad | `spring-security-oauth2-jose` (solo la librería, no el starter completo — a propósito, para que Security no bloquee toda la app) |
| Test | `spring-boot-starter-test`, `spring-boot-webmvc-test`, `spring-modulith-starter-test`, `spring-boot-testcontainers` + `testcontainers-postgresql` + `testcontainers-junit-jupiter`, `archunit-junit5`, `jqwik`, `wiremock-standalone` |

## DI / composition root

Inyección por constructor es la convención (idiomática de Spring); no se detectó
inyección por campo. `@ConfigurationPropertiesScan` en la clase principal
(`BaseConocimientoApplication`) — no hace falta registrar cada `@ConfigurationProperties`
a mano. La configuración está dispersa en `@Configuration` pequeños por módulo (ej.
`SeguridadConfig`, `WebConfig`), no en una clase central única. `@EnableScheduling`
habilita las 4 tareas `@Scheduled` del sistema (relevo de fuentes, worker de embeddings,
recálculo de `term_stats`, etc.).

## Fronteras de módulos (Spring Modulith)

Paquete raíz `co.g3a.baseconocimiento`, anotado `@Modulithic(systemName =
"base-conocimiento")`. Cada subpaquete directo es un módulo; 8 de los 9 declaran
`@org.springframework.modulith.ApplicationModule` en su `package-info.java` para fijar
un `displayName`. El noveno, `seguridad`, es módulo igual por convención (subpaquete
directo del raíz) pero sin `package-info.java` propio.

| Módulo | displayName | Responsabilidad |
|---|---|---|
| `compartido` | Compartido | Vocabulario de dominio compartido; no depende de nadie |
| `ingesta` | Ingesta | Conectores, chunking, cola de trabajo, embebido |
| `llm` | LLM | Cliente de generación (OpenAI contra `llama-server`/Bonsai, Ollama para el destilador) |
| `modelos` | Modelos | Embeddings (Ollama) y cross-encoder ONNX |
| `orquestacion` | Orquestacion | Pipeline de 7 etapas; expone la fachada `Consultar` |
| `recuperacion` | Recuperacion | 4 señales, RRF, cross-encoder |
| `web` | Web | Adaptador HTML/JS: REST, SSE, estáticos |
| `teams` | Teams | Adaptador Bot Connector |
| `seguridad` | *(sin `displayName`)* | Filtro de token Bearer del API programático |

**La regla que hace cumplir `ArquitecturaTest`** (ArchUnit + `ApplicationModules.of(...)
.verify()`, corre en cada `./mvnw test`): `web` y `teams` solo pueden llegar a
`orquestacion.Consultar` (la única puerta) y a `compartido` — nunca directo a
`recuperacion`, `ingesta`, `modelos` ni `llm`. A la inversa, el núcleo (`orquestacion` /
`recuperacion` / `ingesta` / `modelos` / `llm`) no puede depender de `web` ni `teams`.
Alcanzar el paquete interno de otro módulo sin pasar por su API pública es justo la
violación que esta prueba detecta.

## Persistencia

`JdbcClient` de Spring, sin ORM — decisión deliberada, no un default (ver los
comentarios en `pom.xml` y en el `package-info.java` de `recuperacion`): un
`VectorStore`/JPA no puede expresar las 4 señales de retrieval fusionadas por RRF con SQL
a mano. Migraciones con Flyway (`spring-boot-flyway` + `flyway-core` +
`flyway-database-postgresql`), archivos en `src/main/resources/db/migration/`
(`V1__esquema.sql` … `V4__streams_en_curso.sql`), aplicadas automáticamente al arrancar
la app (`spring.flyway.enabled: true`). Detalle del esquema en
[`data-model.md`](data-model.md).

## Configuración y perfiles

Un solo `application.yml`, sin perfiles Spring por ambiente (no hay
`application-{profile}.yml`) — la variación entre entornos es 100% por variables de
entorno con default embebido en cada `${VAR:default}`. `.env.example` es la lista
canónica; se copia a `.env` (nunca commiteado) y Docker Compose lo inyecta al contenedor
`api`. No hay secrets manager: todo vía `.env` / variables de entorno del contenedor.

## Build, ejecución, pruebas

El `Makefile` es la convención de la casa — prefiérelo sobre invocar Maven a mano:

```bash
make build   # ./mvnw -B clean package -DskipTests
make test    # ./mvnw -B test           -- incluye los gates de arquitectura (ArquitecturaTest)
make verify  # ./mvnw -B clean verify
```

No hay separación unit/integration (sin Failsafe, sin `*IT.java`): Surefire corre todo
bajo `**/*Test.java`, `**/*Tests.java`, `**/*Properties.java` (este último para
property-based tests con jqwik). `make test` levanta Testcontainers (Postgres real) — no
hay un `make test-unit` más liviano.

Frameworks: JUnit 5 + AssertJ (vía `spring-boot-starter-test`), Mockito, ArchUnit 1.4.2,
jqwik 1.10.1 (property-based), Testcontainers 2.0.5 (Postgres real en pruebas),
WireMock 3.13.2 (dobla Bot Framework / Graph / Azure DevOps).

`./mvnw spring-boot:run` no es el camino habitual para correr la app completa: espera
Postgres, Ollama y, según el perfil, `llama-server`/`docling-serve` — usa `make up` en su
lugar. Ver [`infrastructure.md`](infrastructure.md).

## Gates de calidad

| Gate | Estado |
|---|---|
| ArchUnit (fronteras de módulos) | ✅ presente — `ArquitecturaTest`, corre en `./mvnw test` |
| Spring Modulith `ApplicationModules.verify()` | ✅ presente — mismo test |
| Checkstyle / Spotless | ❌ ausente |
| SpotBugs / PMD | ❌ ausente |
| SonarQube | ❌ ausente (sin `sonar-project.properties`) |

Solo ArchUnit — sin formatter ni linter estático configurado.

## Superficie web / API

REST puro (`@RestController`), sin GraphQL ni gRPC. 7 controladores: `IngestaController`,
`AdminController`, `ContenidoVaultController` (módulo `ingesta`), `OrquestacionController`
(`/api/ask`, `/api/chat`), `RecuperacionController` (`/api/search`), `ChatController`
(web/SSE), `BotController` (Teams, `/api/messages`). Sin `springdoc-openapi` ni Swagger —
no hay OpenAPI generado. `spring-boot-starter-validation` está presente para `@Valid` en
los DTOs de entrada.

## Empaquetado y despliegue

Jar por capas habilitado (`spring-boot-maven-plugin` → `<layers><enabled>true</enabled>
</layers>`) — reconstruir solo repone la capa de aplicación, no las dependencias.
`Dockerfile` multi-etapa (`deps` → `build` → `layers` → `runtime`) con cache mount de
BuildKit para `~/.m2`. Sin GraalVM native-image. Ver [`infrastructure.md`](infrastructure.md)
para el resto del empaquetado (perfiles de Compose, `Dockerfile.bonsai`).

## Transversales

- **Observabilidad**: `spring-boot-starter-actuator`, endpoints `health,info,metrics`
  expuestos (`management.endpoints.web.exposure.include`), sin backend externo de
  Micrometer cableado.
- **Concurrencia**: hilos virtuales habilitados (`spring.threads.virtual.enabled: true`)
  — el `Executor` de `orquestacion` corre las 6 herramientas de búsqueda en paralelo
  sobre ellos.
- **IA**: `spring-ai-*` (Ollama + API compatible con OpenAI) es el runtime de la app;
  `.mcp.json` en la raíz es instrumentación de Claude Code, no parte del runtime.
- **Mensajería**: ninguna (sin Kafka/RabbitMQ/JMS) — la cola de ingesta (`ingest_jobs`)
  es una tabla Postgres con `SELECT … FOR UPDATE SKIP LOCKED`, no un broker.

## Gotchas

- **Versiones gestionadas por BOM**: no agregues `<version>` a una dependencia ya
  cubierta por uno de los 4 BOMs importados.
- **Sin unit/integration split**: `make test` corre todo, incluidos los tests con
  Testcontainers (Postgres real).
- **`allowEmptyShould(true)` en `ArquitecturaTest`**: las 3 reglas de fronteras lo traen
  puesto porque nacieron antes de que `web`/`teams` existieran (fases F4/F5 del plan).
  Los paquetes guardados ya tienen clases reales hoy — la regla ya se aplica de verdad,
  el flag solo tolera el caso vacío sin ocultar violaciones. No es un bug, pero se puede
  retirar si se quiere que un futuro módulo vacío haga fallar el build en vez de pasar
  en silencio.
- **`seguridad` sin `package-info.java`**: sigue siendo módulo Modulith (subpaquete
  directo del raíz), solo que sin `displayName` propio.

## Docs relacionados

- [`architecture.md`](architecture.md) — pipeline funcional completo, esquema de
  contenedores
- [`data-model.md`](data-model.md) — esquema de datos y migraciones
- [`infrastructure.md`](infrastructure.md) — perfiles de despliegue, Docker Compose
- [`adrs/`](adrs) — decisiones de diseño
