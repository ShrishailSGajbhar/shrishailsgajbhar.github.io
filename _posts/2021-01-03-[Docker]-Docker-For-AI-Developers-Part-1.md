---
layout: post
title: "[Docker] Docker for AI developers: Basics (Part-I)"
tags: ["Docker"]
comments: true
---
In this blog series, I will encapsulate my learning about Docker, an essential skill for all kinds of developers nowadays. In this blog, we will learn about basics of Docker.

`Docker` is a DevOps enabling tool/engine that runs `containers`. It can help maintaining the Dev and Ops teams conflicting goal requirements. Containers make application deployment easy by making "go-to production" even normal, frequent and completely mastered event. Update events also become easy since its possible to have  many containers serving a single application for increased stability during updates [1]. As long as your application is containerized, the Ops team only need to handle your container images without caring for developers hosting environment, dependencies etc. Docker can solve dependency conflicts among different applications hosted on the same server by containerizing each application with its own dependencies where changes made in one container do not affect the other. Docker containers also help in scaling when a single server isn't enough to handle a single application. Docker containers simplify the server upgrading process.

Before going into details of docker, lets consider why developers should consider it as an essential skill. 

A meme famous in developers community is "It works on my machine"

<div style="text-align: center"><img src="/assets/images/20210103/12.jpeg" title="" alt="" width="500"></div>

A funny way to elaborate how docker was born is the following image

<div style="text-align: center"><img src="/assets/images/20210103/11.jpeg" title="" alt="" width="500"></div>

There are different editions of Docker.

1. Docker Desktop (suitable for developer)

2. Docker Engine Community (suitable for small server, small applications)

3. Docker Engine Enterprise (suitable for critical applications)

## Installing Docker Desktop on Ubuntu:

I have Ubuntu 20.04 OS on my local machine hence followed the process given on [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/) to install the Docker.


### Testing "Hello World" of Docker

You can check your installation by typing the following command on your terminal:

```bash
docker run hello-world
```

If you are running this for the first time fresh after docker installation, then the above command will pull an `image` from the [Docker Hub](https://hub.docker.com/) and will display the output

![](/assets/images/20210103/12.png)



## Basic Concepts:

There are 3 basic concepts while learning Docker: **containers, images, and registries**.

`Container` is like a brand new isolated or virtual machine which we want to customize to create our application and host inside the docker server.

Typically a container is isolated from the other containers and even the host OS. It cannot see the other containers, physical storage, or get incoming connections unless you explicitly state that it can. It contains everything it needs to run: OS, packages,
runtimes, files, environment variables, standard input, and output. [1]

Basically a Docker server is a host for many containers. Multiple instances of the same container image can be hosted on the same Docker server such as "App2" in image below.
<div style="text-align: center"><img src="/assets/images/20210103/13.png" title="" alt="" width="270"></div>


`Image` is the template to create the container. Container is created from an **image**. Similar to class concept in object oriented programming, everything needed to create the `container` is defined in an `image`.

<div style="text-align: center"><img src="/assets/images/20210103/15.png" title="" alt="" width="450"></div>

`Registries` store the `images`. In the example above, the App2 image is used
to create two containers. Each container lives its own life, and they both share
a common root: their image from the registry.

### Revisiting the Hello-World of Docker:

Now that, we know the basic concepts of Docker, let us analyze what happened when we ran `docker run hello-world`. This command instructed Docker to create and run a container based on the `hello-world` image assumed to be available locally on our computer. However, since this image is not available, it is downloaded from the  default-registry [Docker Hub](https://hub.docker.com/) and then a container based on the hello-world image is created and run to display the text output followed by stopping of the container.

When we run again the command `docker run hello-world` since the image is already available on our local machine, downloading of image step is skipped in this case.

### Useful container management commands

| Command          | Description                                                 |
| ---------------- | ----------------------------------------------------------- |
| `docker ps`      | lists the containers that are still running.                |
| `docker ps -a`   | Also shows containers that have stopped                     |
| `docker logs ID`    | retrieves the logs of a container, even when it has stopped |
| `docker inspect ID` | gets detailed information about a running/stopped container |
| `docker stop ID`    | deletes a container that is still running                   |
| `docker rm ID`      | deletes a container                                         |

In the above table, `ID` referes to the containter ID which is unique for each container.

Output for first two container management commands is as follows
![](/assets/images/20210103/16.png)
For `docker ps` output is not shown since no containers are currently running. For the second command, we can see the container with ID `ea31855f065f` created from `hello-world` image ran and exited (or stopped) 24 minutes ago. Output for `docker logs` and `docker inspect` commands are as follows:
![](/assets/images/20210103/17.png)
![](/assets/images/20210103/18.png)

If we run the command `sudo docker rm ea31855f065f` will delete the stopped container and running the `docker logs ea31855f065f` shows
![](/assets/images/20210103/19.png) 

# References:

[1] Learn Docker - .NET Core, Java, Node.JS, PHP or Python by Arnaud Weil, 2018

