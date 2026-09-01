make verificar

== 0. Los contenedores estan arriba?
-------------------------------------------------------------------
kb-api  Up 13 seconds (healthy)
kb-db   Up 34 seconds (healthy)
kb-docling-serve        Up 34 seconds (healthy)
kb-ollama       Up 34 seconds (healthy)

== 1. El arreglo de recuperacion esta EN EL CONTENEDOR?
-------------------------------------------------------------------
Esperado: TOPE=20 y EXPANDIR_VECINOS=false. Si sale 3, o no sale nada,
el contenedor es viejo aunque el repo este al dia: make down; make up
  KB_RECUPERACION_TOPE_POR_DOCUMENTO=20
  KB_EXPANDIR_VECINOS=false

== 2. Hay contenido, y esta embebido?
-------------------------------------------------------------------
chunks sin embeber => no es recuperacion, es el worker (TrabajadorEmbebido).
chunks = 0 => no hay corpus: 'make seed' (ver vault-init en el Makefile).
   id |      documento      | chunks | embebidos | pendientes
  ----+---------------------+--------+-----------+------------
    2 | jls25.pdf           |   1060 |      1060 |          0
    3 | muestra-docling.pdf |      4 |         4 |          0
    1 | despliegue.md       |      1 |         1 |          0
  (3 rows)


== 3. RECUPERACION aislada del LLM  --  /api/search
-------------------------------------------------------------------
Este es el paso que decide. /api/search devuelve los candidatos crudos:
sin planificador, sin verificador de grounding y sin sintesis.

  Vuelven fragmentos del documento que responde -> la recuperacion esta
  bien y el problema es de JUICIO (paso 4, y mira techo-confianza).
  Vuelve vacio -> el problema es de BUSQUEDA: sigue en la recuperacion.

  Pregunta: cuales son los tipos primitivos en Java
  SIN RESPUESTA util de /api/search:
    Error en el servidor remoto: (500) Error interno del servidor.
  Ojo: que /actuator/health responda NO descarta esto -- /api/search
  embebe la consulta, y si el modelo de embeddings no esta, se cuelga.
  Modelos que reporta la api:
    disponibles: bge-m3:latest, gemma3:4b
    faltantes  :
    accion     : ninguna

== 4. Que decidio el sistema en tus consultas reales
-------------------------------------------------------------------
candidatos > 0 con citas = 0 => el material llego y algo lo rechazo:
es Orquestador:388, VerificadorGrounding. El perfil base usa
techo-confianza 8.0; Bonsai lo bajo a 6.0 justo por esto (un match de
rerank 7.9 caia en AMBIGUO y se rechazaba). Prueba entonces:
  $env:KB_UMBRAL_RELEVANCIA_TECHO_CONFIANZA='6.0'; make up
   id |                pregunta                | candidatos | citas |             respuesta              | latency_ms
  ----+----------------------------------------+------------+-------+------------------------------------+------------
   17 | tipos primitivos                       |          0 |     0 | No encontr├® informaci├│n suficiente |      23039
   16 | tipos primitivos                       |          0 |     0 | No encontr├® informaci├│n suficiente |        364
   15 | cu├íles son los tipos primitivos en jav |          0 |     0 | No encontr├® informaci├│n suficiente |      17584
   14 | cu├íles son los tipos primitivos en jav |          0 |     0 | No encontr├® informaci├│n suficiente |      11011
   13 | cu├íles son los tipos primitivos en jav |          0 |     0 | No encontr├® informaci├│n suficiente |      19570
  (5 rows)


===================================================================
Si el paso 3 devuelve el documento correcto y el 4 muestra candidatos
con 0 citas, NO toques la recuperacion: el problema es el umbral.
===================================================================
