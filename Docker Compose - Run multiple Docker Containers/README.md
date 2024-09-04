## Write Docker Compose file to run MongoDB and MongoExpress containers
- Refer to ``TWN_Demos_Module7/js-app`` for the project code
- docker-compose.yaml -> needed to automate running commands for multiple applications
- Docker compose will take care of creating a common network for all the container configured
- Start the containers with docker-compose:
  ``docker-compose -f docker-compose.yaml up``
