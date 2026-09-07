PS D:\git_public\base-conocimiento-sandbox> git branch --show-current
dev
PS D:\git_public\base-conocimiento-sandbox> git log -1 --format='%h %ci %s'
66dbf2f 2026-09-03 19:04:40 -0500 El reformulador propone estrategias con pistas del corpus, y la consulta se puede editar (#34)
PS D:\git_public\base-conocimiento-sandbox> git status --short
PS D:\git_public\base-conocimiento-sandbox> docker image inspect base-conocimiento-api:latest --format '{{.Created}}'
2026-09-05T19:31:22.475189189Z
PS D:\git_public\base-conocimiento-sandbox> docker inspect kb-api --format '{{index .Config.Labels "com.docker.compose.project.working_dir"}}'
template parsing error: template: :1: function "com" not defined
