docker general
==

**prepare it**

    Edit .env and set both DOCKER_UID and DOCKER_GID to your user-id and group-id.
    Your user-id and group-id can be looked up by calling 'id'-command on a terminal.

**run shell interactive with mounted volume from docker compose-file**

    docker compose run --rm o4d_mini /bin/bash

**execute another bash in running container**

    docker compose exec -u container -it <container_id> /bin/bash
    where <container_id> can be looked up by calling 'docker ps'.

docker build image
==


**build image**
    
    edit docker-compose.yaml and docker/Dockerfile and call:
    docker compose build


**run container as user root executing bash**

    docker compose run -u root --rm /bin/bash 
