make verificar: comando make up (se supone que usa por defecto gemma3 y no bonsai)

== 0. Los contenedores estan arriba?
-------------------------------------------------------------------
kb-api  Up About a minute (healthy)
kb-ollama       Up About a minute (healthy)
kb-docling-serve        Up About a minute (healthy)
kb-db   Up About a minute (healthy)

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
  fragmentos devueltos: 10

  documento                     rerank   rango
  jls25.pdf                       9.89       1
  jls25.pdf                       9.77       2
  jls25.pdf                       6.04       3
  jls25.pdf                       5.46       4
  jls25.pdf                       4.17       5
  jls25.pdf                       3.01       6
  jls25.pdf                       1.24       7
  jls25.pdf                       1.04       8
  jls25.pdf                       0.88       9
  jls25.pdf                       0.51      10

  -> Llego material. Compara el rerank con techo-confianza (paso 1):
     por debajo del techo la respuesta cae en AMBIGUO y la decide
     VerificadorGrounding, que es donde se rechaza contenido valido.

== 4. Que decidio el sistema en tus consultas reales
-------------------------------------------------------------------
candidatos > 0 con citas = 0 => el material llego y algo lo rechazo:
es Orquestador:388, VerificadorGrounding. El perfil base usa
techo-confianza 8.0; Bonsai lo bajo a 6.0 justo por esto (un match de
rerank 7.9 caia en AMBIGUO y se rechazaba). Prueba entonces:
  $env:KB_UMBRAL_RELEVANCIA_TECHO_CONFIANZA='6.0'; make up
   id |                pregunta                | candidatos | citas |             respuesta              | latency_ms
  ----+----------------------------------------+------------+-------+------------------------------------+------------
   17 | tipos primitivos                       |          0 |     0 | No encontré información suficiente |      23039
   16 | tipos primitivos                       |          0 |     0 | No encontré información suficiente |        364
   15 | cuáles son los tipos primitivos en jav |          0 |     0 | No encontré información suficiente |      17584
   14 | cuáles son los tipos primitivos en jav |          0 |     0 | No encontré información suficiente |      11011
   13 | cuáles son los tipos primitivos en jav |          0 |     0 | No encontré información suficiente |      19570
  (5 rows)


===================================================================
Si el paso 3 devuelve el documento correcto y el 4 muestra candidatos
con 0 citas, NO toques la recuperacion: el problema es el umbral.
===================================================================
