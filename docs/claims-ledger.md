# Registro de afirmaciones

Generado originalmente por la skill `agent-context-java`. Registra las afirmaciones factuales clave
de la documentación, su fuente en el repositorio y si fueron confirmadas por una persona. Vuelve a
ejecutar la skill para regenerarlo.

**Última verificación a mano: 2026-08-31**, leyendo el código. Cuatro afirmaciones habían dejado de
ser ciertas: dos por la sincronización con el monorepo del workshop, y **dos que ya eran falsas
cuando se registraron** (Checkstyle/Spotless y el pipeline de CI ya existían en este repositorio).
Se marcan abajo en vez de borrarlas, porque saber qué dejó de valer vale tanto como saber qué vale.

## Vigentes

| Afirmación | Fuente | Confianza | Estado |
|---|---|---|---|
| El build usa Maven con el wrapper `./mvnw` commiteado. | `mvnw`, `pom.xml` | alta | confirmada |
| El JDK objetivo es 25, alineado entre `pom.xml` y el `Dockerfile` (`eclipse-temurin:25`). | `pom.xml` (`<java.version>`), `Dockerfile` (deps, build y runtime) | alta | confirmada |
| La persistencia es `JdbcClient` sin ORM (no JPA/Hibernate) — decisión deliberada para expresar 4 señales de retrieval fusionadas por RRF en SQL a mano. | `pom.xml` (sin `data-jpa` ni `hibernate-core`), `recuperacion/package-info.java` | alta | confirmada |
| Las migraciones de Flyway se aplican automáticamente al arrancar la app, no en un paso separado de CI. | `application.yml`, `pom.xml` (`spring-boot-flyway`) | alta | confirmada |
| Spring Modulith trata 9 subpaquetes directos de `co.g3a.baseconocimiento` como módulos; `ApplicationModules.verify()` corre en cada `./mvnw test`. | `BaseConocimientoApplication.java`, `ArquitecturaTest.java`, 9 `package-info.java` | alta | confirmada |
| No hay separación unit/integration en las pruebas: Surefire corre todo bajo `**/*Test.java`/`**/*Tests.java`/`**/*Properties.java`, sin Failsafe. | `pom.xml` (configuración de Surefire) | alta | confirmada |
| No hay `springdoc-openapi`/Swagger — sin OpenAPI generado. | `pom.xml` (dependencia ausente) | alta | confirmada |
| Observabilidad limitada a Actuator (`health,info,metrics`), sin Micrometer con backend externo cableado. | `application.yml`, `pom.xml` (sin `micrometer-registry-*`) | alta | confirmada |
| Formato, estilo, arquitectura y CI bloquean el build; SpotBugs/PMD y SonarQube siguen ausentes. | `pom.xml` (Spotless, Checkstyle `failOnViolation=true`), `.github/workflows/ci.yml` | alta | confirmada (2026-08-31) |
| `ArquitecturaTest` tiene 5 pruebas: 4 de ArchUnit (5 `noClasses()`) más `ApplicationModules.verify()`, con `allowEmptyShould(false)` en las de adaptadores, núcleo y `seguridad`. | `ArquitecturaTest.java` | alta | confirmada (2026-08-31) |
| `seguridad` es módulo Modulith explícito, con `@ApplicationModule` y Javadoc propios, y está cubierto por las reglas en las dos direcciones más su frontera lateral con `web`/`teams`. | `seguridad/package-info.java`, `ArquitecturaTest.seguridadNoSeMezclaConLosOtrosAdaptadores` | alta | confirmada (2026-08-31) |
| `jqwik` está fijado en 1.9.3 a propósito: 1.10.x imprime una inyección de prompt contra agentes en cada corrida. | `pom.xml` (comentario de `<jqwik.version>`), <https://lwn.net/Articles/1075317/> | alta | confirmada (2026-08-31) |
| El `Makefile` fija su propio `SHELL` en Windows (el `sh.exe` de Git for Windows) y le antepone su directorio al `PATH` cuando el `PATH` viene en formato Windows. | `Makefile` (bloque `ifeq ($(OS),Windows_NT)`) | alta | confirmada (2026-08-31) — sin eso, `make` desde PowerShell cae a `cmd.exe` y casi ninguna receta funciona |
| El reparto de la GPU se deriva de `nvidia-smi` (VRAM, Compute Capability, driver), no de constantes. | `Makefile` (`GPU_PLAN`), `make gpu-check` | alta | confirmada (2026-08-31) |
| `docling-serve` no libera la VRAM entre conversiones y `GET /v1/clear/converters` no la recupera; solo reiniciar el proceso. | Medido: 2053 MiB antes y después del endpoint; sesión 27 de `investigacion-vram-y-modelo-llm.md` | alta | confirmada (2026-08-31) |
| Hay 10 `compose.*.yml` de perfil de modelo; 7 tienen target `up-`/`down-`/`pull-`. | `ls compose.*.yml`, `grep "^up-" Makefile` | alta | confirmada (2026-08-31) |
| El despliegue de producción es self-hosted vía Docker Compose en un único host, sin nube. | `README.md` ("Costo cero… sin nube") + ausencia de manifiestos de Kubernetes/Terraform | media | TODO: verificar — hay CI, pero **no** hay CD |

## Invalidadas por cambios posteriores

| Afirmación (ya no vigente) | Qué la invalidó |
|---|---|
| «No hay Checkstyle, Spotless, SpotBugs, PMD ni SonarQube configurados — solo ArchUnit.» | Checkstyle y Spotless están en el `pom` y bloquean el build. **Esta afirmación ya era falsa cuando se registró**: se marcó como confirmada sin contrastarla contra el `pom`. SpotBugs, PMD y SonarQube sí siguen ausentes. |
| «No hay pipeline de CI/CD en el repo (sin `.github/workflows/`…).» | `.github/workflows/ci.yml` existe y corre `make ci` en cada push/PR. **También era falsa de antes.** Lo que no hay es CD. |
| «El paquete `seguridad` … no tiene `package-info.java`/`@ApplicationModule` propio.» | Se trajo del monorepo del workshop, con `@ApplicationModule(displayName = "Seguridad")` y Javadoc. |
| «Las 3 reglas de fronteras de `ArquitecturaTest` traen `allowEmptyShould(true)`.» | La fusión de las dos versiones del test dejó `allowEmptyShould(false)` en las reglas de adaptadores, núcleo y `seguridad`; solo `compartidoEsHoja` conserva `true`. Y ya no son 3 reglas, son 5 pruebas. |

## Notas

- Las afirmaciones de confianza "alta" se verificaron leyendo el archivo citado o confirmando la
  ausencia del archivo/dependencia correspondiente.
- La lección de las dos filas «ya era falsa de antes»: una afirmación marcada `confirmada` sin citar
  la línea concreta que la sostiene envejece mal y nadie la vuelve a mirar.
- La única marcada TODO es la topología de producción: el repo no tiene una carpeta de despliegue
  separada de la de desarrollo, así que "producción = mismo Compose" es una inferencia razonable
  pero no confirmada explícitamente por una persona.
