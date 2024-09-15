## Create Docker hosted repository on Nexus
- Settings > Repositories > New repository > docker (hosted) > blob store: mystore

## Create Docker repository role on Nexus
- In order to push docker images to the docker repository, we need to log into nexus using a nexus user which has access to nexus docker repository.
- Settings > Security > Roles > Create Role:
  - Type: Nexus role
  - Role ID: nx-docker
  - Role Name: nx-docker
  - Privileges: nx-repository-view-docker-hosted-*
- Settings > Security > Users > Assign the newly created role (nx-docker) to your existing user

## Configure Nexus, DigitalOcean Droplet and Docker to be able to push to Docker repository
- Docker Login to Nexus Docker Repo:
  - Create a port on Nexus for the docker repository:
    - Settings > Repositories > docker-hosted > HTTP (because we already connect to Nexus with a Not secure connection)
    - set a port in the HTTP field (eg: 8083)
    - This means that the repository will be accessible at the ip address of nexus + the port we have configured (8083)
  - Open the port on the droplet firewall configuration:
    - Digital Ocean > Networking > my-droplet-firewall > new inbound rule (custom, TCP, 8083, All IPv4 All IPv6)
  - Configure Realm in Nexus
    - When we do docker login, we get a token of authentication from Nexus for a client. This token will be stored in the local machine in docker/config.json (this is needed to avoid having to use our credentials for every command we run when communicating to the docker repository after having already logged in)
    - The issuing of this token can be configured in Nexus in Settings > Security > Realms
    - Docker Bearer Token Realm > add it to the active list
  - Configure Docker client to allow the docker repository as an insecure registry
    - By default, docker allows only client requests to talk to an https endpoint (a secure endpoint)
    - In a Linux environment, add the following line to /etc/docker/daemon.json:
      ``"insecure-registries" : ["http://nexusipaddress:8083"]``
    - In Windows, go to Docker Desktop > Settings > Docker Engine > edit the configuration file (after the line starting with "experimental")
    - Apply and restart Docker Desktop

## Build and Push Docker image to Docker repository on Nexus
- Docker login:
  - ``docker login nexusipaddress:8083``
  - Add username and password of your nexus user (the one with nx-docker role)
- Build image
  - ``cd my-app/`` -> move to the directory that contains the docker file
  - ``docker build -t my-app:1.0 .``
  - Re-tag image to include the address of the repository itself: ``docker tag my-app:1.0 . nexusipaddress:8083:/my-app:1.0``
- Push image to Nexus repo
  - ``docker push nexusipaddress:8083:/my-app:1.0``
- Fetch Docker Image from Nexus
  - ``curl -u nexusUsername:nexusPassword -X GET 'nexusipaddress:8081/service/rest/v1/components?repository=docker-hosted``
