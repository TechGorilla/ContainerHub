## Table of Contents 

- [Table of Contents](#table-of-contents)
  - [UTILS](#utils)
- [GO](#go)
- [DOTNET](#dotnet)
- [JAVA](#java)
  - [Maven](#maven)
- [JAVASCRIPT](#javascript)
  - [AngularJS](#angularjs)
  - [Deno](#deno)
  - [NestJS](#nestjs)
  - [NodeJS](#nodejs)
  - [ReactJS](#reactjs)
  - [Svelte](#svelte)
  - [VueJS](#vuejs)
- [PHP](#php)
  - [Laravel](#laravel)
- [PYTHON](#python)
  - [Django](#django)

### UTILS
NGINX nginx config file:
```xml
worker_processes 1;

events {
    worker_connections 1024;
}

http {
    server {
        listen 80;
        server_name localhost;

        root /usr/share/nginx/html;
        index index.html index.htm;
        include /etc/nginx/mime.types;

        gzip on;
        gzip_min_length 1000;
        gzip_proxied expired no-cache no-store private auth;
        gzip_types text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/*;

        location / {
            try_files $uri $uri/ /index.html;
        }

    }
}
```


## GO
Create a simple go app:
```powershell
# initialize
go mod init example.com/hello  
# add dependency
go get github.com/gofiber/fiber/v2
```

Create a "main.go" file.
```go
package main

import "github.com/gofiber/fiber/v2"

func main() {
	app := fiber.New()

	app.Get("/", func(c *fiber.Ctx) error {
		return c.SendString("Hello Go!")
	})
    // you choose the port to listen on
	app.Listen(":80")
}
```
Test the program.
```powershell
# run example
go run .
```

**Dockerfile**
```docker
FROM golang:1.16-alpine

WORKDIR /go_app
COPY go.mod .
COPY go.sum .
RUN go mod download
COPY . .
RUN go build -o ./out/dist .
CMD ./out/dist
```

**Usage:**
```powershell
# build the image
docker build t [IMAGE] .
# run the container and map its port 80
docker run -p [hostport]:80 [IMAGE]
```
Go to a browser and the app should be running on: *localhost:[hostport]*.


## DOTNET

Create a simple dotnet app:
```powershell
# initialize the project
dotnet new console -o [appName]
# run the app
cd [appName]
dotnet run
# build 
dotnet publish -c Release -o out
```

**Dockerfile**
```Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:5.0 as build

WORKDIR /dotnet_app
COPY *.csproj . 
RUN dotnet restore 
COPY . .

RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet
WORKDIR /dotnet_app
COPY --from=build /dotnet_app/out .
# replace [appName] buy the appName you chose
ENTRYPOINT ["dotnet", "[appName].dll"]
```

**Usage:**
```powershell
# build the image
docker build t [IMAGE] .
# run the container and map its port 80
docker run -p [hostport]:80 [IMAGE]
```
The output should be displayed in the terminal.


## JAVA

### Maven 

*  **Maven Testing container** \
This container accepts a maven GOAL during runtime. The Dockerfile is to be placed in the directory where the "pom.xml" and "src/" are located.

**Dockerfile**
```Dockerfile
# Base image
FROM openjdk:8-jdk-alpine
# Install Maven
RUN apk add --no-cache curl tar bash
ARG MAVEN_VERSION=3.6.3
ARG USER_HOME_DIR="/root"
RUN mkdir -p /usr/share/maven && \
curl -fsSL http://apache.osuosl.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xzC /usr/share/maven --strip-components=1 && \
ln -s /usr/share/maven/bin/mvn /usr/bin/mvn
ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"
# Speed up Maven JVM a bit
ENV MAVEN_OPTS="-XX:+TieredCompilation -XX:TieredStopAtLevel=1"
# Turn the image into a maven builder, you just need to append a maven goal to the container at runtime
ENTRYPOINT ["/usr/bin/mvn"]
# Install project dependencies and keep sources
# Make app folder
WORKDIR /usr/java_apps/
# Copy the POM.XML to the WORKDIR
COPY pom.xml ./
# Copy the src directory
COPY src ./src
```
**Usage:**
```powershell
docker build -t [IMAGE] .
# Build the container image
docker run [IMAGE] [Maven_Goal]
# ex: docker run [IMAGE] test
```

## JAVASCRIPT

### AngularJS

Create a simple AngularJS app:
```powershell
# install the angular CLI 
npm i -g @angular/cli
# create angular project directory
ng new [appName]
# build the Project
cd [appName]
ng serve 
# default port is 4200
# Edit package.json file
# add to the "scripts" dictionary
# "prod": "ng build --configuration production"
# build the project
npm run prod 
```

Create nginx.conf file (check UTILS section)

**Dockerfile**
```Dockerfile
FROM node:15.4-alpine as build

WORKDIR /angular_app
COPY package*.json .
RUN npm install
COPY . .

RUN npm prod


FROM nginx:1.19
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY --from=build /angular_app/dist/angular/ /usr/share/nginx/html
```

**Usage:**
```powershell
# build the image
docker build t [IMAGE] .
# run the container and map its port 80
docker run -p [hostport]:80 [IMAGE]
```
Go to a browser and the app should be running on: *localhost:[hostport]*.

### Deno

Create simple deno app:\
In the project folder add a [appName].ts file
```ts
// contents of the sample app
const listener = Deno.listen({ port: 80 });
console.log("http://localhost:80/");
for await (const conn of listener) {
  (async () => {
    const requests = Deno.serveHttp(conn);
    for await (const { respondWith } of requests) {
      respondWith(new Response("Hello Deno!"));
    }
  })();
}
```
Run the app:
```powershell
deno run --allow-net [appName].ts 
```

**Dockerfile**
```Dockerfile
FROM python:3.9-alpine

ENV PYTHONUNBUFFERED 1

WORKDIR /python_app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD python manage.py runserver 0.0.0.0:80
```

**Usage:**
```powershell
# build the image
docker build t [IMAGE] .
# run the container and map its port 80
docker run -p [hostport]:80 [IMAGE]
```
Go to a browser and the app should be running on: *localhost:[hostport]*.

### NestJS

Create a simple nestJS app:
```powershell
# install the nest CLI 
npm i -g @nestjs/cli
# create nest project directory
nest new [appName]
# build the Project
cd [appName]
npm run start
# in the src/ directory you can change the port number from the default (3000) in the main.ts file
## change the port number from this bit:  app.listen(3000);
```
Create a .dockerignore file to reduce the size
```sh
# These elements will only bloat the container
node_modules
npm-debug.log
build
.dockerignore
**/.git
**/node_modules
```

**Dockerfile**
```dockerfile
FROM node:16.6-alpine as build

WORKDIR /nest_app
COPY package*.json .
RUN npm install
COPY . .
RUN npm run build 

FROM node:16.6-alpine
WORKDIR /nest_app
COPY package.json .
RUN npm install --only=production
COPY --from=build /nest_app/dist ./dist
CMD npm run start:prod
```

**Usage:**
```powershell
# build the image
docker build t [IMAGE] .
# run the container and map its port 80
docker run -p [hostport]:80 [IMAGE]
```
Go to a browser and the app should be running on: *localhost:[hostport]*.

### NodeJS

Create a simple nodeJS app:
```powershell
# check if node is installed
nodejs -v
# check if npm is installed
npm -v
# init the project in a [folder] 
npm init
# install express
npm i express
# ls should return
package.json package-lock.json
```
You need to add an index file: 

**index.js**
```js
const express = require('express');
const app = express();
// print a simple text message to the browser
app.get('/', function(req, res){
    res.send("Hello NODE!");
});
// we set the port to use on the container
app.listen(80);
```
Create a .dockerignore file to reduce the size
```sh
# These elements will only bloat the container
node_modules
npm-debug.log
build
.dockerignore
**/.git
**/node_modules
```

**Dockerfile**
```Dockerfile
# Base image
FROM node:16.6-alpine
# Copy package files
COPY package*.json .
# Run command
RUN npm install
# Copy files from pwd to the default directory in the container
COPY . .
# launch the app
CMD node index.js
```
**Usage:**
```powershell
# build the image
docker build t [IMAGE] .
# run the container and map its port 80
docker run -p [hostport]:80 [IMAGE]
```
Go to a browser and the app should be running on: *localhost:[hostport]*.

### ReactJS

Create a simple reactJS app:
```powershell
# create the directory
npx create-react-app [appName]
# navigate in
cd [appName]/
# initiate the app
npm start
```

Create a .dockerignore file to reduce the size
```sh
# These elements will only bloat the container
node_modules
npm-debug.log
build
.dockerignore
**/.git
**/node_modules
```

**Dockerfile**
```Dockerfile
# pull official base image
FROM node:16.6-alpine
# set working directory
WORKDIR /react_app
# add `/react_app/node_modules/.bin` to $PATH
ENV PATH /react_app/node_modules/.bin:$PATH
# install app dependencies
COPY package.json ./
COPY package-lock.json ./
RUN npm install
# add app
COPY . ./
# start app
CMD ["npm", "start"]   
```

**Usage:**
```powershell
# build the [IMAGE]
docker build -t [IMAGE] .
# run the [CONTAINER]
docker run -it --rm \
-v ${PWD}:/app \
-v /app/node_modules \
-p [hostPort]:3000 \
-e CHOKIDAR_USEPOLLING=true \
[IMAGE]
```
Go to a browser and the app should be running on: *localhost:[hostport]*.


### Svelte

Create a simple svelte app:
```powershell
# use the svelte kit
npm init svelte@next [appName]
cd [appName]
npm install
# run the svelte app
npm run dev -- --open
```

Create nginx.conf file (check UTILS section)

**Dockerfile**
```Dockerfile
FROM node:16.6-alpine as build

WORKDIR /svelte_app
COPY package*.json .
RUN npm install 
COPY . .
RUN npm run build

FROM nginx:1.19
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY --from=build /svelte_app/public /usr/share/nginx/html
```

**Usage:**
```powershell
# build the image
docker build t [IMAGE] .
# run the container and map its port 80
docker run -p [hostport]:80 [IMAGE]
```
Go to a browser and the app should be running on: *localhost:[hostport]*.

### VueJS

Create a simple vueJS app:
```powershell
# install the Vue CLI 
npm install -g @vue/cli
# solve vulnerability issues
npm i joi
# check if vue is installed
vue -V
# create vue project directory
vue create [appName]
# build the Project
cd [appName]
npm run build
```
Troubleshooting: \
*You may get an error when running the script in powershell (Windows OS).* \
Simply set the execution policy:
```powershell
# this will allow you to run the script
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

(Extra) Create an app.vue file:
**[app].vue**
```js
<template>
    <div>
        <h2>Hello VUE!</h2>
        <marquee>It works!</marquee>
    </div>
</template>
```
To launch this app run the command:
```powershell
vue serve [app].vue
```
Create nginx.conf file (check UTILS section)

Create a .dockerignore file to reduce the size
```sh
# These elements will only bloat the container
node_modules
npm-debug.log
build
.dockerignore
**/.git
**/node_modules
```

**Dockerfile**
```Dockerfile
# Multistage build
# Build stage
FROM node:16.6-alpine as build
# set the work directory
WORKDIR /vue_app
# dependencies 
COPY package*.json .
RUN npm install
# create the build
COPY . .
RUN npm run build

# set the web server
FROM nginx:1.19
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY --from=build /vue_app/dist /usr/share/nginx/html
```

**Usage:**
```powershell
# build the image
docker build t [IMAGE] .
# run the container and map its port 80
docker run -p [hostport]:80 [IMAGE]
```
Go to a browser and the app should be running on: *localhost:[hostport]*.

## PHP

### Laravel

## PYTHON

### Django

Create a simple Django app:
```powershell
# install django
python -m pip install Django
# create a project 
django-admin startproject [appName]
# run the project
python manage.py runserver
```

Create a requirements.txt file
```sh
# contents of the requirements.txt file
Django==3.1.7
```

**Dockerfile**
```Dockerfile
FROM python:3.9-alpine

ENV PYTHONUNBUFFERED 1

WORKDIR /python_app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD python manage.py runserver 0.0.0.0:80
```

**Usage:**
```powershell
# build the image
docker build t [IMAGE] .
# run the container and map its port 80
docker run -p [hostport]:80 [IMAGE]
```
Go to a browser and the app should be running on: *localhost:[hostport]*.


