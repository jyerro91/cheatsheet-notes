# Docker Workflow


## Tagging

docker image built -t <name>:<tag>
or
docker image built --t <name>:<tag>

<name> = Org, Group, or Project
<tag> = Ideally the latest git commit hash

Use the commit hash GIT as the image tag

git log -1 --pretty=%H

Use docker image tag to create a new tagged image

docker image tag <source_image>:<tag> <target_image>:<tag>

docker image tag hps/proservice:hash hps/proservice:latest


