# Day 01 --- Docker Fundamentals: Containers, Images & Registries

## Learning goals

By the end of this session, you should be able to:

-   Explain what Docker and containers are.
-   Understand the difference between an image and a container.
-   Run, inspect, and stop a container.
-   Understand port mapping.
-   Find and download images from a registry.
-   Build, tag, and push an image to Docker Hub.

------------------------------------------------------------------------

## 1. What is Docker?

Docker is a platform used to package and run applications in containers.

A container includes the application and the files, libraries,
dependencies, and configuration it needs to run. This helps the
application behave consistently across development, testing, and
production environments.

**Simple example:** An Express application works on your laptop but
fails on a server because the server has a different Node.js version or
missing dependencies. Docker helps package the required environment so
the application can run consistently.

### Why use Docker?

1.  **Consistency:** Run the application in a predictable environment.
2.  **Isolation:** Keep application environments separate from one
    another.
3.  **Portability:** Run containers on different machines and cloud
    environments.
4.  **Easy deployment:** Package the application and its requirements
    together.
5.  **Scaling:** Run multiple instances of an application when more
    capacity is needed.

> Containers are isolated processes. They share the host operating
> system's kernel; they are not complete virtual machines.

------------------------------------------------------------------------

## 2. What is a container?

A **container** is an isolated running process created from an image. It
runs an application with the files and configuration it needs.

A real-world application may use separate containers for:

-   React frontend
-   Node.js / Express backend
-   MongoDB database

Each component can run in its own container.

### Container benefits

-   **Self-contained:** Includes what the application needs to run.
-   **Isolated:** Has its own process and filesystem view.
-   **Independent:** Containers can be managed separately.
-   **Portable:** Can run in different supported environments.

### Container vs Virtual Machine

A virtual machine runs a complete guest operating system with its own
kernel. Containers share the host kernel and isolate application
processes, so they generally have less overhead than a full VM.

------------------------------------------------------------------------

## 3. What is a Docker image?

A **Docker image** is a packaged, read-only template used to create
containers.

An image can include:

-   Application code
-   Runtime (for example, Node.js)
-   Required libraries and dependencies
-   Files and configuration
-   Instructions for starting the application

### Image vs Container

  -----------------------------------------------------------------------
  Docker Image                        Docker Container
  ----------------------------------- -----------------------------------
  Packaged template                   Created from an image

  Used to create containers           Runs the application process

  Image layers are immutable          Has a writable runtime layer

  One image can create many           Each container is an individual
  containers                          instance
  -----------------------------------------------------------------------

**Remember:** Image = template; Container = running instance of that
template.

An image can create multiple containers. For example, one backend image
can be used to start `backend-1` and `backend-2`.

A container does not necessarily disappear if its original image is
later removed. Existing containers have their own container filesystem
layer. However, you need an image to create a new container.

------------------------------------------------------------------------

## 4. What is a Docker registry?

A **registry** is a service that stores and distributes Docker images.

Examples:

-   Docker Hub --- public registry and Docker's default registry.
-   Amazon Elastic Container Registry (ECR)
-   Azure Container Registry (ACR)
-   Google Artifact Registry (GAR)

A registry can be public or private.

### Registry vs Repository vs Image

  -----------------------------------------------------------------------
  Term                                Meaning
  ----------------------------------- -----------------------------------
  Registry                            The service that stores and
                                      distributes images

  Repository                          A named collection of related
                                      images in a registry

  Image                               A specific packaged application
                                      template, often identified by a tag
  -----------------------------------------------------------------------

Think of a registry as a library, a repository as a shelf/folder, and
images/tags as the packaged versions stored there.

### Important terms

-   **Pull:** Download an image from a registry to your machine.
-   **Push:** Upload an image from your machine to a registry.
-   **Tag:** Give an image a name/version label.
-   **Login:** Authenticate with a registry.

------------------------------------------------------------------------

## 5. Port mapping

A container can run a web server on a port inside the container. To
access it from the host machine, publish that port.

Example:

``` bash
docker run -d -p 8080:80 docker/welcome-to-docker
```

`-p HOST_PORT:CONTAINER_PORT`

-   `8080` = port on your laptop (host).
-   `80` = port inside the container.
-   Open `http://localhost:8080` in your browser.

**Important:** Publishing a port is not required for every container. A
command-line script may not need a port. A web application usually needs
a published port if you want to access it from the host.

For a Node.js Express app listening on container port `3000`:

``` bash
docker run -d -p 8080:3000 my-backend
```

The app must listen on `0.0.0.0` inside the container to be reachable
through the published port; listening only on the container's
`localhost` may prevent access from outside that container.

------------------------------------------------------------------------

## 6. Docker commands --- Containers

### Run a container

``` bash
docker run -d -p 8080:80 docker/welcome-to-docker
```

-   `docker run` --- create and start a container from an image.
-   `-d` --- run in detached/background mode.
-   `-p 8080:80` --- publish host port 8080 to container port 80.
-   `docker/welcome-to-docker` --- image name.

If the image is not available locally, Docker will try to pull it from
the configured registry.

### List running containers

``` bash
docker ps
```

Shows containers that are currently running.

### List all containers, including stopped ones

``` bash
docker ps -a
```

### Stop a container

First find its ID or name:

``` bash
docker ps
```

Then stop it:

``` bash
docker stop <container-id-or-name>
```

Example:

``` bash
docker stop welcome-to-docker
```

A stopped container still exists. Stopping it is not the same as
deleting it.

------------------------------------------------------------------------

## 7. Docker commands --- Images

### Search Docker Hub for an image

``` bash
docker search docker/welcome-to-docker
```

### Download (pull) an image

``` bash
docker pull docker/welcome-to-docker
```

Another example:

``` bash
docker pull node:22
```

`node:22` means the Node.js image with the `22` tag.

### List local images

``` bash
docker image ls
```

Short form:

``` bash
docker images
```

### View image layers and history

``` bash
docker image history docker/welcome-to-docker
```

Add `--no-trunc` to display full commands:

``` bash
docker image history --no-trunc docker/welcome-to-docker
```

### Run a container from an image

``` bash
docker run node:22
```

This creates and starts a container from the Node.js 22 image. It does
not publish a port. A port is only needed when a service inside the
container must be reached through a host port.

------------------------------------------------------------------------

## 8. Build your own image

A Dockerfile contains instructions for building an image.

From the directory that contains your Dockerfile, run:

``` bash
docker build -t my-backend .
```

-   `docker build` --- build an image.
-   `-t my-backend` --- assign the image a name/tag.
-   `.` --- use the current directory as the build context (the location
    Docker can access for the build).

List the image:

``` bash
docker images
```

> Dockerfile details and building a MERN application will be practised
> in the upcoming hands-on sessions.

------------------------------------------------------------------------

## 9. Tag and push an image to Docker Hub

These are the basic steps to publish your own image.

### Step 1 --- Sign in

``` bash
docker login
```

Follow the browser/device-code instructions if Docker displays them. Do
not share your password or access token.

### Step 2 --- Tag your image

Replace `YOUR_DOCKER_USERNAME` with your Docker Hub username:

``` bash
docker tag my-backend YOUR_DOCKER_USERNAME/my-backend:1.0
```

A tag helps identify a particular image version. For example, `1.0`,
`1.1`, or `latest`.

### Step 3 --- Push the image

``` bash
docker push YOUR_DOCKER_USERNAME/my-backend:1.0
```

The image is now uploaded to the matching Docker Hub repository,
assuming the repository exists and you have permission to push to it.

### Step 4 --- Log out (optional)

``` bash
docker logout
```

------------------------------------------------------------------------

## 10. Complete workflow to remember

``` text
Dockerfile + Application Code
            |
            | docker build
            v
      Docker Image
            |
            | docker run
            v
    Docker Container
            |
            | docker tag
            v
   Registry-ready Image
            |
            | docker push
            v
       Docker Hub
            |
            | docker pull (on another machine)
            v
     New Docker Container
```

------------------------------------------------------------------------

## 11. Quick command reference

  Command                               Purpose
  ------------------------------------- -----------------------------------
  `docker run IMAGE`                    Create and start a container
  `docker run -d IMAGE`                 Run a container in the background
  `docker run -p 8080:80 IMAGE`         Publish a container port
  `docker ps`                           List running containers
  `docker ps -a`                        List all containers
  `docker stop CONTAINER`               Stop a container
  `docker search TERM`                  Search Docker Hub
  `docker pull IMAGE`                   Download an image
  `docker image ls` / `docker images`   List local images
  `docker image history IMAGE`          View image layers/history
  `docker build -t NAME .`              Build an image from a Dockerfile
  `docker login`                        Sign in to a registry
  `docker tag SOURCE TARGET`            Add a tag/name to an image
  `docker push IMAGE`                   Upload an image to a registry
  `docker logout`                       Sign out of a registry

------------------------------------------------------------------------

## 12. Day 01 practical task

Run these commands one by one in your terminal:

``` bash
# 1. Search for the sample image
docker search docker/welcome-to-docker

# 2. Download the image
docker pull docker/welcome-to-docker

# 3. Check downloaded images
docker image ls

# 4. Run the container
docker run -d -p 8080:80 --name welcome-demo docker/welcome-to-docker

# 5. Check the running container
docker ps

# 6. Open this address in your browser
# http://localhost:8080

# 7. View all containers
docker ps -a

# 8. Stop the container
docker stop welcome-demo
```

### Expected outcome

-   You can open the Welcome to Docker page at `http://localhost:8080`.
-   You can identify the image and container in `docker ps`.
-   You can stop the container successfully.

### Self-test

1.  What is Docker?
2.  What is the difference between an image and a container?
3.  Can one image create multiple containers?
4.  What does `docker ps -a` show that `docker ps` does not?
5.  What does `-p 8080:80` mean?
6.  What is a Docker registry?
7.  What is the difference between `docker pull` and `docker push`?
8.  Why do we use `docker tag`?

------------------------------------------------------------------------

## Official Docker documentation

-   [What is a
    container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)
-   [What is an
    image?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/)
-   [What is a
    registry?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-registry/)
-   [Docker CLI
    reference](https://docs.docker.com/reference/cli/docker/)
-   [Docker Hub](https://hub.docker.com/)

**Day 01 takeaway:** A registry stores images, an image is a packaged
template, and a container is an instance created from an image.
