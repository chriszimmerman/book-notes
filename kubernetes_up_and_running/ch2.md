# Chapter 2 - Creating and Running Containers

*Container image* - binary package that encapsulates all of the files necessary to run a program inside of an OS container

Container registries store images

Docker filesystem

Overlay filesystem - each layer adds, removes, or modifies files from the preceding layer in the filesystem

Example:

```
.
└── container A: a base operating system only, such as Debian
    └── container B: build upon #A, by adding Ruby v2.1.10
    └── container C: build upon #A, by adding Golang v1.6
```

Containers fall into two categories:
- System containers - mimic VM
- Application containers - commonly run a single program

## Building Application Images With Docker

Dockerfiles - automate creation of a Docker image

*.dockerignore*

```
node_modules
```

*.Dockerfile*

```dockerfile
# Start from a Node.js 10 (LTS) image
FROM node:10

# Specify the directory inside the image in which all commands will run
WORKDIR /usr/src/app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy all of the app files into the image
COPY . .

# The default command to run when starting the container
CMD [ "npm", "start" ]
```

Build a `simple-node` Docker image:

`docker build -t simple-node .`

Run image:

`docker run --rm -p 3000:3000 simple-node`

Don't build containers with passwords baked in

## Multistage Image Builds

A dockerfiler can produce multiple stages

Single vs Multi-stage:

Single

```dockerfile
FROM golang:1.11-alpine

# Install Node and NPM
RUN apk update && apk upgrade && apk add --no-cache git nodejs bash npm

# Get dependencies for Go part of build
RUN go get -u github.com/jteeuwen/go-bindata/...
RUN go get github.com/tools/godep

WORKDIR /go/src/github.com/kubernetes-up-and-running/kuard

# Copy all sources in
COPY . .

# This is a set of variables that the build script expects
ENV VERBOSE=0
ENV PKG=github.com/kubernetes-up-and-running/kuard
ENV ARCH=amd64
ENV VERSION=test

# Do the build. This script is part of incoming sources.
RUN build/build.sh

CMD [ "/go/bin/kuard" ]
```

Multi-stage

```dockerfile
# STAGE 1: Build
FROM golang:1.11-alpine AS build

# Install Node and NPM
RUN apk update && apk upgrade && apk add --no-cache git nodejs bash npm

# Get dependencies for Go part of build
RUN go get -u github.com/jteeuwen/go-bindata/...
RUN go get github.com/tools/godep

WORKDIR /go/src/github.com/kubernetes-up-and-running/kuard

# Copy all sources in
COPY . .

# This is a set of variables that the build script expects
ENV VERBOSE=0
ENV PKG=github.com/kubernetes-up-and-running/kuard
ENV ARCH=amd64
ENV VERSION=test

# Do the build. Script is part of incoming sources.
RUN build/build.sh

# STAGE 2: Deployment
FROM alpine

USER nobody:nobody
COPY --from=build /go/bin/kuard /kuard

CMD [ "/kuard" ]
```

Multi-stage builds can reduce final container image size significantly, resulting in the speedup of deployment times

## Storing Images in Remote Registry

Private vs public registries

Tag image:

`docker tag kuard gcr.io/kuar-demo/kuard-amd64:blue`

Push image:

`docker push gcr.io/kuar-demo/kuard-amd64:blue`

## Docker Container Runtime

CRI API is implemented by a number of programs, including `containerd` and `cri-o`

Deploy container:

`docker run -d --name kuard \
  --publish 8080:8080 \
  gcr.io/kuar-demo/kuard-amd64:blue`

Limit memory usage:

`docker run -d --name kuard \
  --publish 8080:8080 \
  --memory 200m \
  --memory-swap 1G \
  gcr.io/kuar-demo/kuard-amd64:blue`

Limit CPU:

`docker run -d --name kuard \
  --publish 8080:8080 \
  --memory 200m \
  --memory-swap 1G \
  --cpu-shares 1024 \
  gcr.io/kuar-demo/kuard-amd64:blue`


## Cleanup

`docker rmi <tag-name>`

`docker rmi <image-id>`

View images:

`docker images`

Cleanup:

`docker system prune`