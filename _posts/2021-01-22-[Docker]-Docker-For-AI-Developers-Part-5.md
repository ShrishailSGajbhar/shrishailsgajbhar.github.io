---

layout: post
title: "[Docker] Docker for AI developers: Using Docker Compose For Machine Learning Applications (Part-V)"
tags: ["Docker","Machine Learning"]
comments: true

---

In the previous blog post, we created our first Dockerfile customized for our machine learning development needs. In this last post on the Docker series, we will try to understand how Docker compose can help us for creating an end-to-end machine learning development environment. 

# What is Docker Compose?

It is a higher-level abstraction tool that can handle multiple containers at a time. It is a tool for defining and running multi-container docker application. It uses YAML or (YML) file to configure  application services which is usually named as `docker-compose.yml (or docker-compose.yaml)`. It also makes spinning up and tearing down the application services very easily with just two simple commands:

* `docker-compose up`: spin up all the services 
* `docker-compose down`: tear down all the services
  
Using Docker compose, we can also scale up the selected services without affecting the other services.

## How to install Docker Compose on Ubuntu

If `docker-compose` is not available then you can install it as

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Finally check installation using `docker-compose --version` at the terminal.

## Why Docker Compose?

Before writing our first Docker compose file, it is worth spending some time on why we should learn the Docker compose? Well it is worth, let's see why:
the three main important phases of any ML application are **build, train and deploy**. Docker compose can help us test all these 3 phases on your local computer so that discrepancies in the development and deployment phases can be minimized. For example, if you use jupyter notebook server as a development tool for your ML application and use Flask, a micro web framework to check deployment of your application in the production environment, then using Docker compose, you can spin up these services whenever you wish as well as tear them down. Another example, let's say you are using jupyter notebook server as a development tool, a MLFlow server to record and organize experiment parameters and metrics and Postgres Database as the backend for the MLflow server and as a handy datastore for your structured datasets (this example is taken from [2]). Here, all three services would be interacting with each other and in such cases Docker compose is very useful which can manage such multi-container applications easily. I hope you have got the gist of why we should learn and use the Docker compose.

## Docker Compose Application Model Terms [3]

1. **services:** Computing components of an application are defined as [Services](https://github.com/compose-spec/compose-spec/blob/master/spec.md#Services-top-level-element). A Service is an abstract concept implemented on platforms by running the same container image (and configuration) one or more times.

2. **networks:** Services communicate with each other through [Networks](https://github.com/compose-spec/compose-spec/blob/master/spec.md#Networks-top-level-element)

3. **volumes:** Services store and share persistent data into [Volumes](https://github.com/compose-spec/compose-spec/blob/master/spec.md#Volumes-top-level-element).

4. **configs:** Some services require configuration data that is dependent on the 
   runtime or platform. For this, the specification defines a dedicated 
   concept: [Configs](https://github.com/compose-spec/compose-spec/blob/master/Configs-top-level-element). 

5. **secrets:** A [Secret](https://github.com/compose-spec/compose-spec/blob/master/spec.md#Secrets-top-level-element) is a specific flavour of configuration data for sensitive data that SHOULD NOT be exposed without security considerations.

## Typical Docker Compose File

Please find below an example of a typical Docker compose file named as `docker-compose.yml` [4].

```yml
version: '3'
services:
  web:
    build: .
    ports:
    - "5000:5000"
  redis:
    image: "redis:alpine"
```

The above file defines two services, a `web` service and a `redis` service. In case of web service, its image is built using the build context provided as the current folder i.e., `.` where the `Dockerfile` is available and in case of `redis`, a database service, the  image will be pulled from the DockerHub as it is specified using `image` attribute.  In case of `web` services the port mapping is  defined using `5000:5000` meaning that port 5000 of the local machine will be mapped to port 5000 inside the container. For more details about the use of above Docker compose file see [5]. 

## Examples of Docker Compose for Machine Learning

One can take a look at [handson-ml/docker · ageron/handson-ml · GitHub](https://github.com/ageron/handson-ml/tree/master/docker) to understand the use of `docker-compose` for running jupyter notebooks. 

This [article](https://towardsdatascience.com/containerize-your-whole-data-science-environment-or-anything-you-want-with-docker-compose-e962b8ce8ce5) beautifully explains the use of docker compose for creating machine learning environment running multiple services.

One can always use GitHub to search for docker-compose examples for machine learning such as [GitHub - kongzii/ml-project-template: Machine learning project template with ready to use docker-compose, mlflow, tensorboard, and other utilities set up.](https://github.com/kongzii/ml-project-template)

# References:

1. https://www.youtube.com/watch?v=HUpIoF_conA
2. https://towardsdatascience.com/containerize-your-whole-data-science-environment-or-anything-you-want-with-docker-compose-e962b8ce8ce5
3. https://github.com/compose-spec/compose-spec/blob/master/spec.md
4. https://docs.docker.com/compose
5. https://docs.docker.com/compose/gettingstarted/
