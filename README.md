# Udagram Image Filtering Microservice

Exercise from "Cloud Developer Nanodegree Program" - "Monolith to Microservices at Scale" in Udacity. The original repo, I opted not to do a fork because I would end up forkign the whole course instead of just this project.

## Description
Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

The project is split into four parts:
1. [The Simple Frontend](/udacity-c3-frontend)
A basic Ionic client web application which consumes the RestAPI Backend. 
2. [The RestAPI Feed Backend](/udacity-c3-restapi-feed), a Node-Express feed microservice.
3. [The RestAPI User Backend](/udacity-c3-restapi-user), a Node-Express user microservice.
4. [Deployment](/udacity-c3-deployment), Kubernetes deployment

## Getting Setup

> _tip_: this frontend is designed to work with the RestAPI backends). It is recommended you stand up the backend first, test using Postman, and then the frontend should integrate.

### Installing Node and NPM
This project depends on Nodejs and Node Package Manager (NPM). Before continuing, you must download and install Node (NPM is included) from [https://nodejs.com/en/download](https://nodejs.org/en/download/).

### Installing Ionic Cli
The Ionic Command Line Interface is required to serve and build the frontend. Instructions for installing the CLI can be found in the [Ionic Framework Docs](https://ionicframework.com/docs/installation/cli).

### Installing project dependencies

This project uses NPM to manage software dependencies. NPM Relies on the package.json file located in the root of this repository. After cloning, open your terminal and run:
```bash
npm install
```
>_tip_: **npm i** is shorthand for **npm install**

### Setup Backend Node Environment
You'll need to create a new node server. Open a new terminal within the project directory and run:
1. Initialize a new project: `npm init`
2. Install express: `npm i express --save`
3. Install typescript dependencies: `npm i ts-node-dev tslint typescript  @types/bluebird @types/express @types/node --save-dev`
4. Look at the `package.json` file from the RestAPI repo and copy the `scripts` block into the auto-generated `package.json` in this project. This will allow you to use shorthand commands like `npm run dev`


### Configure The Backend Endpoint
Ionic uses enviornment files located in `./src/enviornments/enviornment.*.ts` to load configuration variables at runtime. By default `environment.ts` is used for development and `enviornment.prod.ts` is used for produciton. The `apiHost` variable should be set to your server url either locally or in the cloud.

***
### Running the Development Server
Ionic CLI provides an easy to use development server to run and autoreload the frontend. This allows you to make quick changes and see them in real time in your browser. To run the development server, open terminal and run:

```bash
ionic serve
```

### Building the Static Frontend Files
Ionic CLI can build the frontend into static HTML/CSS/JavaScript files. These files can be uploaded to a host to be consumed by users on the web. Build artifacts are located in `./www`. To build from source, open terminal and run:
```bash
ionic build
```
***

## Deployment

### Setup - Docker
#### Dependencies
- Postgres SQL
- AWS S3

#### Configure

```bash 
# Database Configurations
export POSTGRESS_USERNAME=<database username>;
export POSTGRESS_PASSWORD=<database password>;
export POSTGRESS_DB=<database schema>;
export POSTGRESS_HOST=<database address>;

# AWS Configurations
export AWS_REGION=<e.g: eu-west-1 >;
export AWS_PROFILE=<>;
export AWS_BUCKET=<>;

# Udagrama Configurations
export JWT_SECRET=<Random generated secret>;
```

### Docker
To build the images:
```bash
docker-compose -f udacity-c3-deployment/docker/docker-compose-build.yaml build --parallel
```

From the folder `udacity-c3-deployment/docker`it is possible to run the containers on your computer using:
```bash
docker-compose up
```

### Setup - Kubernetes

### Configure
Everything is done in the context of `udacity-c3-deployment/k8s`

The following files should be configured:
- aws-secret.yaml;
- env-configmap.yaml;
- env-secret.yaml;

### Startup kubernetes! 
```bash 

# Deploy Configurations!
    kubectl apply -f aws-secret.yaml
    kubectl apply -f env-secret.yaml
    kubectl apply -f env-configmap.yaml

# Deploy  
# Apply Deployments!

    kubectl apply -f backend-feed-deployment.yaml
    kubectl apply -f frontend-deployment.yaml
    kubectl apply -f backend-user-deployment.yaml
    kubectl apply -f reverseproxy-deployment.yaml

# Apply Services!

    kubectl apply -f backend-feed-service.yaml
    kubectl apply -f backend-user-service.yaml
    kubectl apply -f frontend-service.yaml
    kubectl apply -f reverseproxy-service.yaml

# Port Forwarding!
    kubectl port-forward service/frontend 8100:8100
    kubectl port-forward service/reverseproxy 8080:8080
```
