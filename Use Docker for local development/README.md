## Create Dockerfile for Nodejs application and build Docker image
- Go to ``TWN_Demos_Module7/js-app`` to see the project code
- ``docker pull mongo`` -> to pull the mongo db image from DockerHub
- ``docker pull mongo-express`` -> to pull the mongo-express image (this is a GUI for Mongo)
- ``docker network create mongo-network`` -> to create a custom network where the containers will run
- Dockerfile -> summarizes most of the information needed to run the application

## Run Nodejs application in Docker container and connect to MongoDB database container locally
- ``node server.js`` to run the Node.js app (available on localhost:3000)
- Start mongo container:
  - ``docker run -d \
     -p 27017:27017 \
      -e MONGO_INITDB_ROOT_USERNAME=admin \
      -e MONGO_INITDB_ROOT_PASSWORD=password \
      --name mongodb \
      --net mongo-network
      mongo``

## Run MongoExpress container as a UI of the MongoDB database
- Start mongo express container:
  - ``docker run -d \
     -p 8081:8081 \
      -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
      -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
      --name mongo-express \
      --net mongo-network \
      -e ME_CONFIG_MONGODB_SERVER=mongodb \
      mongo-express``
- Configurations to connect the Nodejs app to mongo db -> see server.js file
- Create database ("user-accounts") + collection ("users") in the Mongo express UI (available on localhost:8081)
