## Create and Configure Droplet
- Create Droplet:
  - Digital Ocean > Create > Regular (8 GB / 4 CPUs) > Create Droplet
- Configure Firewall:
  - Add droplet to the already existing firewall
- Log in to droplet

## Set up and run Nexus as Docker container
- Update package manager -> ``apt update``
- Install latest version of docker -> ``snap install docker``
- Configure volumes so that nexus knows where to save its data -> ``docker volume create --name nexus-data``
- Install nexus and start nexus container -> ``docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3``
