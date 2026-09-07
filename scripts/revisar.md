git branch --show-current
git log -1 --format='%h %ci %s'
git status --short
docker image inspect base-conocimiento-api:latest --format '{{.Created}}'
docker inspect kb-api --format '{{index .Config.Labels "com.docker.compose.project.working_dir"}}'
