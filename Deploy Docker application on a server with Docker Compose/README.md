**Note**: this is the final version of the app at the end of this demo: *TWN_Demos_Module7/Deploy Docker application on a server with Docker Compose/js-app*

## Copy Docker-compose file to remote server
- Prod scenario: we need to pull my-app image from AWS and the mongo-db and mongo-express image from Docker-Hub into a server
- In the docker-compose.yaml set the full name for the image of "my-app" (replace the content that has curly brackets)
- ``vim docker-compose.yaml`` -> to create a copy of the docker-compose.yaml in the server (within *my-app* folder)

## Login to private Docker registry on remote server to fetch our app image
- ``docker login --username AWS --password-stdin 454342277166.dkr.ecr.eu-central-1.amayonaws.com`` -> to login onto AWS ECR repository

## Start our application container with MongoDB and MongoExpress services using docker compose
- ``docker-compose -f docker-compose.yaml up``
- In server.js, set the url for mongo (no need to use localhost:port, but instead use the name of the container, like "mongodb")
  **NB**: after you do this, you need to rebuild the image and re-push it into AWS ECR repo
