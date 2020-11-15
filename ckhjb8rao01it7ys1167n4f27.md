## How To Dockerize A Simple Django App

## Introduction
According to Docker docs, 
*Docker is a tool that provides OS-level virtualization to deliver software in packages called containers.*

 Docker made application containers easy to use by providing a simplified, opinionated wrapper around existing Linux distribution invoked as containers, process control, security, and resource management technologies.

Docker excels in these two key areas:
-  Packaging applications
-  Process isolation and management

> *Docker helps you package and run applications inside isolated little boxes with all platform and application dependencies provided, thus keeping the hosting computer nice and tidy.*

Docker solves problems associated with :
- Missing or incorrect application dependencies such as libraries, interpreters, code/binaries, users, ubuntu packages, etc 
- Conflicts between programs running on the same host machine such as library dependencies or ports; Example: local Django trying to use port 80
- Insufficient system resources required to run an application such as CPU and memory.

Let’s see how Docker delivers on its promise to “Build, Ship, and Run” applications easily 
😃.

**Assumptions**:  We'll be working with a [Django application](https://www.django-rest-framework.org/), Some experience working with a Django application might be needed and you have [Docker](https://docs.docker.com/get-docker/) & [Docker Compose](https://docs.docker.com/compose/) installed.

## Packaging an Application Into An Image
This OS-level virtualization provided by [Docker](https://docs.docker.com/get-docker/) helps us mirror the application and platform into a ```docker file```.
The ```docker file``` should be placed at the root of the Django project.

```
# The first instruction is what image we want to base our container on
# We Use an official Python runtime as a parent image
FROM python:3.8.5 as production

# The environment variable ensures that the python output is set straight
# to the terminal without buffering it first
ENV PYTHONUNBUFFERED 1

# create a root directory for our project in the container
RUN mkdir /app

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app/

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# tell the port number the container should expose
EXPOSE 8000

```
Docker images also allow you to define metadata and define more options to help operators run the application according to your application needs.

## Setting Up A Docker-compose.yml File

For clarity, Docker Compose solves the problem of running multi-container applications at once. You thus can set the desired amount of containers counts, their builds, services, and volumes, and then with a single set of commands, you can build, run, and configure all the containers.

> According to [Docker-compose Docs](https://docs.docker.com/compose/)
Using Compose is basically a three-step process:  

 -  Define your app’s environment with a Dockerfile so it can be reproduced anywhere.

 - Define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment.

-  Run docker-compose up and Compose starts and runs your entire app.

it is also located in the root directory of the Django application.
The Docker environment variable file ```.env``` is necessary when you're creating complex containers or Database service for deployments.

So let's say you have your ```.env file``` (that's the full name of the file, by the way) located in the same directory that houses your ```docker-compose.yml file```. In the ```.env file```, you have on these lines: 

```
DATABASE_NAME=dbase
DATABASE_USER=postgresuser
DATABASE_PASSWORD=postgresuser
DATABASE_HOST=localhost
DATABASE_ENGINE=django.db.backends.postgresql
DATABASE_PORT=5432

```

A simple ```docker-compose.yml``` looks like this:

```
version: '3.8'

# This file defines two services: The db service and the web service.
services:
  db:
    image: postgres
    environment:
# key-value pair for the environmental variable
      - environment:
          - POSTGRES_USER: "${DATABASE_USER}"
          - POSTGRES_DB: "${DATABASE_NAME}"
          - POSTGRES_PASSWORD: "${DATABASE_PASSWORD}"
          - POSTGRES_HOST: "${DATABASE_HOST}"
    volumes:
# no data is lost  when we stop running the container, only for development
      - ./pgdata:/var/lib/postgresql/data
    ports:
# exposing port 5432 to the host machine
      - "5432:5432"
  web:
    build: .
    command: > 
        bash -c "
        python manage.py makemigrations
        && python manage.py migrate 
        && python manage.py runserver 0.0.0.0:8000
        "
    image: appserver
# for development purposes, so when we make changes to the source code the change gets saved to the container.
    volumes:
      - .:/appserver
    depends_on:
      - db
    ports:
# expose port 80 to the host machine
      - "8000:80"

```

We're all set now, lets build and run our container using this simple  command :
```
docker-compose up
```

That’s a simple  introduction to how application containers can help your team solve many common packaging, distribution, operational problems, and ship better software 
🤓.

Thanks for the audience and I hope you found this article helpful 🤗. Feel free to reach out to me on  [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.

  ### Further Reading
- [Docker-compose documentation](https://docs.docker.com/compose/)
- [Docker documentation](https://docs.docker.com/engine/reference/builder/)
- [Install docker on ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04)

