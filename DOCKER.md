# Docker
Note:  kubia is just the name of our simple image

## Run an image and optionally pass it a command (downloading it from docker repo if necessary)
`docker run busybox echo "Hello world"`

Have a look what's publicly available here: http://hub.docker.com  
login as: `gillianbc` pwd is in keychain

`docker run <image>`

## Dockerfile:
```
FROM node:7
ADD app.js /app.js
ENTRYPOINT ["node", "app.js"]
```

## Build a local image called kubia based on  Dockerfile instructions in current folder
`docker build -t kubia .`

## List Local Docker Images
`docker images`

## Run me up a Container called x on port y using image w
`docker run --name kubia-container -p 8080:8080 -d kubia`
* name the container kubia-container
* run it on local port 8080 mapped to 8080 in the container
* detach it from console (run in background)

## Examining running containers
`docker ps`

## Inspect a container
`docker inspect kubia-container`

## Execute Bash Shell Inside Container
`docker exec -it kubia-container bash`
* execute
* i = allow input
* t = pseudo terminal
exit will exit bash within the container and you're back in your computer

You can use ps, ps -ef or ps aux to view running processes
---
## Stopping and Removing a Container
`docker stop kubia-container`

`docker rm kubia-container`

## Tagging
`docker tag kubia gillianbc/kubia`
* add a tag to the kubia image with tagname gillianbc/kubia

## Push Container to Docker Hub
`docker login`
gillianbc   xxxxxx
docker push gillianbc/kubia
