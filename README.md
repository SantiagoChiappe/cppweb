# cppweb
 Web Servers and APIs using C++ course

## Creating container
On folder /hello_crow/bbox
docker build --rm --squash --no-cache -t bbox:latest .

docker run -v ~Location:/usr/src/cppweb -ti bbox:latest bash

## Creating hello_crow
Dockerfile has been updated to build by itself (Run docker image section)

Older version :
cd /usr/src/cppweb/hello_crow/build
cmake ..
build


## Image and app to run
docker run -v ~Location:/usr/src/cppweb -p 8080:8080 -e PORT=8080 cppbox:latest /usr/src/cppweb/hello_crow/build/hello_crow

hello_crow is the app to run. Is the executable cmake built.


## Containerize app
Run the container -> docker run -ti cppbox:latest bash
Get container id -> docker ps -> copy id -> docker cp . <id>:/usr/src/cppweb
docker commit <id> hello_crow:latest

## Deploy docker image to DockerHub
Get image id -> docker images -> copy id
docket tag <id> <username>/hello_crow:latest
docker push <username>/hello_crow

## Run DockerHub image
docker run -p 8080:8080 -e PORT=8080 <username>/hello_crow

Don't know if launching the app is necessary. Just in case:
docker run -p 8080:8080 -e PORT=8080 <username>/hello_crow /usr/src/cppweb/hello_crow/build/hello_crow

Or even
docker run -v ~Location:/usr/src/cppweb -p 8080:8080 -e PORT=8080 <username>/hello_crow /usr/src/cppweb/hello_crow/build/hello_crow

## Connecting app to db
docker build --rm --squash --no-cache -t hello_crow_v2:latest .    

Railway : 
docker run -p 8080:8080 -e PORT=8080 -e MONGODB_URI="<database>://<username>:<password>@<railwayProjectAddress>" hello_crow:latest
railwayProjectAddress is given by Railway

MongoDB Atlas : 
docker run -p 8080:8080 -e PORT=8080 -e MONGODB_URI="<database>://<username>:<password>@<mongodbAddress>" hello_crow:latest
mongodbAddress is given by MongoDB