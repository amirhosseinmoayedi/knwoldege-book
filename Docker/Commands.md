#docker #commands 

More Complete version of [Docker Cheate Sheet](https://dockerlabs.collabnix.com/docker/cheatsheet/)
| Command                                                                        | Description                                                                                            |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `docker build -t <image_name> .`                                             | Build an image using a Dockerfile                                                                      |
| `docker commit <container_id> <image_name>`                                 | Build an image from a container                                                                        |
| `docker load -i <file_name>.tar`                                             | Build an image from a Tar archive                                                                      |
| `docker run -d <image_name>`                                                 | Run a container in the background                                                                      |
| `docker run -p <host_port>:<container_port> <image_name>`                   | Run a container and map host ports to container ports                                                   |
| `docker run -v <host_directory>:<container_directory> <image_name>`         | Run a container with a volume                                                                          |
| `docker ps`                                                                   | List running containers                                                                                |
| `docker stop <container_id>`                                                 | Stop a running container                                                                                |
| `docker rm <container_id>`                                                   | Remove a container                                                                                     |
| `docker images`                                                               | List Docker images                                                                                     |
| `docker rmi <image_id>`                                                      | Remove an image                                                                                        |
| `docker tag <image_id> <image_name>:<tag_name>`                             | Tag an image                                                                                           |
| `docker push <image_name>:<tag_name>`                                        | Push an image to a registry                                                                            |
| `docker pull <image_name>:<tag_name>`                                        | Pull an image from a registry                                                                          |
| `docker exec -it <container_id> <command>`                                  | Execute a command in a running container                                                               |
| `docker logs <container_id>`                                                 | Get logs from a container                                                                              |
| `docker exec -it <container_id> /bin/bash`                                  | Get a shell in a running container                                                                     |
| `docker cp <host_path> <container_id>:<container_path>`                     | Copy files/directories from host to container                                                          |
| `docker cp <container_id>:<container_path> <host_path>`                     | Copy files/directories from container to host                                                          |
| `docker network create <network_name>`                                       | Create a new network                                                                                   |
| `docker network connect <network_name> <container_id>`                       | Connect a container to a network                                                                       |
| `docker network disconnect <network_name> <container_id>`                    | Disconnect a container from a network                                                                  |
| `docker network ls`                                                           | List networks                                                                                           |
