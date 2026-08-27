# Registro de afirmaciones

Generado por la skill `agent-context-java`. Registra las afirmaciones factuales clave de
la documentación, su fuente en el repositorio y si fueron confirmadas por una persona.
Vuelve a ejecutar la skill para actualizarlo.

| Afirmación | Fuente | Confianza | Estado |
|---|---|---|---|
| El build usa Maven con el wrapper `./mvnw` commiteado. | `mvnw`, `pom.xml` | alta | confirmada |
| El JDK objetivo es 25, alineado entre `pom.xml` y el `Dockerfile` (`eclipse-temurin:25`). | `pom.xml:24`, `Dockerfile:2,30,49` | alta | confirmada (usuario: corrigió `pom.xml` de 21 a 25 para alinearlo con el Dockerfile) |
| La persistencia es `JdbcClient` sin ORM (no JPA/Hibernate) — decisión deliberada para expresar 4 señales de retrieval fusionadas por RRF en SQL a mano. | `pom.xml:90-94`, `recuperacion/package-info.java` | alta | confirmada |
| Las migraciones de Flyway se aplican automáticamente al arrancar la app, no en un paso separado de CI. | `application.yml:28-30` | alta | confirmada |
| Spring Modulith trata 9 subpaquetes directos de `co.g3a.baseconocimiento` como módulos; `ApplicationModules.verify()` corre en cada `./mvnw test`. | `BaseConocimientoApplication.java`, `ArquitecturaTest.java` | alta | confirmada |
| El paquete `seguridad` es módulo Modulith por convención (subpaquete directo) pero no tiene `package-info.java`/`@ApplicationModule` propio. | listado de archivos de `src/main/java/.../seguridad/` | alta | confirmada |
| Las 3 reglas de fronteras de `ArquitecturaTest` traen `allowEmptyShould(true)`, y los paquetes que guardan (`web`, `teams`) ya contienen clases reales — el flag ya no esconde nada, solo tolera el caso vacío. | `ArquitecturaTest.java` + listado de `web/`, `teams/` | alta | confirmada |
| No hay separación unit/integration en las pruebas: Surefire corre todo bajo `**/*Test.java`/`**/*Tests.java`/`**/*Properties.java`, sin Failsafe. | `pom.xml:250-260` | alta | confirmada |
| No hay Checkstyle, Spotless, SpotBugs, PMD ni SonarQube configurados — solo ArchUnit. | búsqueda de archivos de config (ausentes) | alta | confirmada |
| No hay `springdoc-openapi`/Swagger — sin OpenAPI generado. | `pom.xml` (dependencia ausente) | alta | confirmada |
| No hay pipeline de CI/CD en el repo (sin `.github/workflows/`, `Jenkinsfile`, `azure-pipelines*.yml`, `.gitlab-ci.yml`). | búsqueda de archivos de CI (ausentes) | alta | confirmada |
| El despliegue de producción es self-hosted vía Docker Compose en un único host, sin nube. | `README.md` ("Costo cero... sin nube") + ausencia de manifiestos de Kubernetes/Terraform | media | TODO: verificar (queda marcado como TODO en `docs/infrastructure.md`) |
| Observabilidad limitada a Actuator (`health,info,metrics`), sin Micrometer con backend externo cableado. | `application.yml:101-108`, `pom.xml` (sin `micrometer-registry-*`) | alta | confirmada |

## Notas

- Todas las afirmaciones de confianza "alta" se verificaron leyendo el archivo citado o
  confirmando la ausencia del archivo/dependencia correspondiente — no son inferencias
  débiles.
- La única marcada TODO es la topología de producción: el repo no tiene una carpeta de
  despliegue separada de la de desarrollo, así que "producción = mismo Compose" es una
  inferencia razonable pero no confirmada explícitamente por una persona.
