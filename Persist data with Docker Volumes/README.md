## Persist data of a MongoDB container by attaching a Docker volume to it
- Define volumes in docker-compose.yaml
- The default path for mondo-db on the drive is:
  - Windows: C:/ProgramData/docker/volumes
  - Linux: /var/lib/docker/volumes
  - MacOS: /var/lib/docker/volumes
- NB: when I restart a container, I delete and create a new container
- To access the folder containing the data on the drive, execute first this command:
  ``docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh`` -> this creates a container that can connect to that directory
  
