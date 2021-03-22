# Deploying a Flask API on AWS

In this project, my job is to containerize and deploy a Flask API to a Kubernetes cluster using Docker, AWS EKS, CodePipeline, and CodeBuild. For the containerization, I wrote a Dockerfile to containerize this app, and then built and tested the container locally. For the deployment, I created an EKS cluster on AWS, a pipeline that can be triggered by GitHub checkins on CodePipeline, as well as a CodeBuild stage that automatically builds, tests and deploys the app.

As the app relies on a secret set as the environment variable `JWT_SECRET` to produce a JWT, I stored the secret using AWS Parameter Store. The built-in Flask server was adequate for local development, but not for production, so I used the production-ready [Gunicorn](https://gunicorn.org/) server when deploying the app.

The Flask app that is used for this project consists of a simple API with three endpoints:

- `GET '/'`: This is a simple health check, which returns the response 'Healthy'. 
- `POST '/auth'`: This takes a email and password as json arguments and returns a JWT based on a custom secret.
- `GET '/contents'`: This requires a valid JWT, and returns the un-encrpyted contents of that token. 

## Dependencies

- Docker Engine
    - Installation instructions for all OSes can be found [here](https://docs.docker.com/install/).
    - For Mac users, if you have no previous Docker Toolbox installation, you can install Docker Desktop for Mac. If you already have a Docker Toolbox installation, please read [this](https://docs.docker.com/docker-for-mac/docker-toolbox/) before installing.
 - AWS Account
     - You can create an AWS account by signing up [here](https://aws.amazon.com/#).
