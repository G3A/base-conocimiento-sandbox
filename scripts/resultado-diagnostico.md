PS D:\git_public\base-conocimiento-sandbox> docker inspect -f '{{json .HostConfig.DeviceRequests}}' kb-ollama
[{"Driver":"nvidia","Count":1,"DeviceIDs":null,"Capabilities":[["gpu"]],"Options":null}]
PS D:\git_public\base-conocimiento-sandbox> docker exec kb-ollama nvidia-smi --query-gpu=name --format=csv,noheader
NVIDIA GeForce RTX 3060 Laptop GPU
PS D:\git_public\base-conocimiento-sandbox> make down
docker compose --env-file .env -f compose.yml -f compose.gpu.yml -f compose.docling-gpu.yml down --remove-orphans
[+] down 5/5
 ✔ Container kb-api                  Removed                                                                                          2.4s
 ✔ Container kb-docling-serve        Removed                                                                                          3.6s
 ✔ Container kb-db                   Removed                                                                                          0.7s
 ✔ Container kb-ollama               Removed                                                                                          0.8s
 ✔ Network base-conocimiento_default Removed                                                                                          0.3s
PS D:\git_public\base-conocimiento-sandbox> make -n up-ministral
log=$(mktemp); est=$(mktemp); ( docker compose --env-file .env -f compose.yml -f compose.gpu.yml -f compose.ministral.yml up -d --build 2>&1; echo $? > "$est" ) | tee "$log"; codigo=$(cat "$est"); if [ "$codigo" != "0" ] && grep -qE "in offline mode and the artifact|has not been downloaded from it before" "$log"; then echo ""; echo "  =================================================================="; echo "  El build fallo porque el cache de Maven quedo vacio, NO por una"; echo "  dependencia rota del proyecto."; echo ""; echo "  La capa de descarga sigue marcada CACHED, asi que no se reintenta"; echo "  sola. Hay que invalidarla:"; echo ""; echo "      make cache-reciclar"; echo "      make up-ministral"; echo ""; echo "  La primera vez despues de eso tarda ~11 min en volver a bajar las"; echo "  dependencias. Las siguientes vuelven a ser segundos."; echo "  =================================================================="; echo ""; fi; rm -f "$log" "$est"; exit $codigo
