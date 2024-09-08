## Write Dockerfile to build a Docker image for a Nodejs application
- See ``TWN_Demos_Module7/js-app`` for the project code
- Dockerfile: it is a blueprint for building docker images of an app
- ``FROM node`` -> install node
- It is possible to set the environmental variables within the Dockerfile, but it is better to set them outside of the image (aka: in the docker-compose.yaml file)
- The ``COPY`` command is executed on the host machine, while the ``RUN`` command is executed within the container
- ``COPY . /home/app`` -> copy everything (.) in the home directoy of the host machine into the container (/home/app)
- ``CMD ["node","server.js"]`` -> start the app with: "node server.js"
- Note that you can have multiple ``RUN`` commands within a Dockerfile, but only one ``CMD`` command
- ``docker build -t myapp:1.0 .`` -> to run the Dockerfile (``-t`` -> tag, ``.`` -> the location of the Dockerfile)
- When we run the Dockerfile, we create the image for the app
- ``docker run my-app:1.0`` -> to start the app
- To delete an image: ``docker rm containerId`` + ``docker rmi imageId`` (we need to delete and recreate an image every time we make changes to the Dockerfile)

## Create private Docker registry on AWS (Amazon ECR)
- Docker registry = create private repository for Docker
- aws.amazon.com > sign in > search for "Elastic Container Registry" > Get Started
- **NB**: On AWS ECR you have one repository per image (but each repo can contain multiple versions of an image)
- push image into AWS repo:
  - AWS CLI needs to be installed (See module 9, lesson 11)
  - AWS Credentials need to be configured
  - create credentials to login to AWS repo from pc:
    ``aws ecr get-login-password --region eu-sentral-1 | docker login --username AWS --password-stdin 454342277166.dkr.ecr.eu-central-1.amayonaws.com`` -> you can find this command on AWS, on the "Push commands for my-app"

## Push Docker image to this private repository
- Tag your image so you can push the image to the repository:
  ``docker tag my-app:latest 454342277166.dkr.ecr.eu-central-1.amazonaws.com/my-app:latest`` -> you can find this command on AWS, on the "Push commands for my-app"
- Push the image to the newly created AWS repository:
  ``docker push 454342277166.dkr.eu-central-1.amazonaws.com/my-app:latest`` -> you can find this command on AWS, on the "Push commands for my-app"
