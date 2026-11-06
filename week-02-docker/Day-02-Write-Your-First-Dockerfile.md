# Day 02 --- Write Your First Dockerfile

**Learning source:** [Docker Docs --- Write your first
Dockerfile](https://docs.docker.com/learn/content/write-your-first-dockerfile/)

**Practice project:**
[docker/getting-started-todo-app](https://github.com/docker/getting-started-todo-app)

## Day 02 goal

By the end of this session, I should be able to:

-   Explain what a Dockerfile does.
-   Understand `FROM`, `WORKDIR`, `COPY`, `RUN`, `EXPOSE`, `CMD`, `ENV`,
    and `.dockerignore`.
-   Build a Docker image from the Todo project's backend.
-   Run a container and test its API.
-   Understand build context and port mapping.
-   Explain why `Cannot GET /` is expected in this exercise.

------------------------------------------------------------------------

## 1. What is a Dockerfile?

A **Dockerfile** is a plain-text file named exactly `Dockerfile` (no
file extension). It contains instructions that Docker follows to build
an image.

A Dockerfile generally does three things:

1.  Selects a base image/runtime.
2.  Copies application files and installs dependencies.
3.  Defines how the application should start.

**Flow:**

`Dockerfile` → `docker build` → **Docker image** → `docker run` →
**Container**

The image contains the application, its dependencies, and the runtime
needed to run it.

## 2. Understand the Todo project structure

The cloned repository contains both a backend and a frontend:

``` text
getting-started-todo-app/
├── backend/
│   ├── package.json
│   ├── package-lock.json
│   └── src/
├── client/
│   ├── package.json
│   └── src/
├── Dockerfile
├── compose.yaml
└── .dockerignore
```

-   `backend/`: Node.js/Express API. It stores Todo items in SQLite.
-   `client/`: React frontend.
-   `Dockerfile`: the repository's original, more advanced Dockerfile.
-   `.dockerignore`: tells Docker which files/folders to leave out of
    the build context.

**Important for this lesson:** We temporarily replace the root
Dockerfile with a simple Dockerfile that builds **only the backend
API**. We are not building the React frontend in this exercise.

To restore the repository's original Dockerfile later:

``` bash
git restore Dockerfile
```

## 3. The Dockerfile used in this lesson

From the repository root, replace the contents of `Dockerfile` with:

``` dockerfile
FROM node:22

WORKDIR /usr/local/app

COPY backend ./

RUN npm ci

EXPOSE 3000

CMD ["node", "src/index.js"]
```

### Line-by-line explanation

  -----------------------------------------------------------------------
  Instruction                         Meaning
  ----------------------------------- -----------------------------------
  `FROM node:22`                      Starts from the Node.js 22 base
                                      image. Node.js and npm are
                                      available inside it.

  `WORKDIR /usr/local/app`            Sets the working directory inside
                                      the image. Docker creates it if it
                                      does not exist.

  `COPY backend ./`                   Copies the contents of the host
                                      project's `backend/` folder into
                                      the current working directory
                                      inside the image.

  `RUN npm ci`                        Installs dependencies during image
                                      build, using the lockfile for a
                                      consistent install.

  `EXPOSE 3000`                       Documents that the app is intended
                                      to listen on container port 3000.
                                      It does **not** publish the port to
                                      the host.

  `CMD ["node", "src/index.js"]`      Sets the default command that runs
                                      when a container starts from this
                                      image.
  -----------------------------------------------------------------------

### Why `COPY backend ./`?

The Dockerfile is in the repository root, and our build context will
also be the repository root.

-   Source: `backend` --- the backend folder in the build context.
-   Destination: `./` --- the current working directory inside the image
    (`/usr/local/app`).

After the copy, the image has paths such as:

``` text
/usr/local/app/
├── package.json
├── package-lock.json
└── src/
    └── index.js
```

The frontend `client/` folder is not copied by this Dockerfile.

### Why `npm ci` instead of `npm install`?

`npm ci` installs the dependency versions recorded in
`package-lock.json`. It is suitable for repeatable builds. It expects a
lockfile to exist.

`npm install` is also commonly used, but the official lesson uses
`npm ci`.

## 4. Build the image

Run this command from the **repository root**, where the Dockerfile is
located:

``` bash
docker build -t my-first-build .
```

Breakdown:

-   `docker build`: build an image.
-   `-t my-first-build`: give the image the name/tag `my-first-build`.
-   `.`: use the current directory as the **build context**.

The build context is the set of files Docker can access for `COPY` and
`ADD`. A `COPY` instruction cannot copy files from outside that context.

Check that the image exists:

``` bash
docker image ls
```

Or:

``` bash
docker images
```

### Build context vs Dockerfile location

These are related but different concepts:

-   `-f` selects the Dockerfile path.
-   The final path argument selects the build context.

For this lesson, both are in the repository root, so
`docker build -t my-first-build .` is sufficient.

If a Dockerfile is in the root but the build context is `./client`, then
`COPY` can only access files inside `client/`. The Dockerfile's location
does not change that.

## 5. What is `.dockerignore`?

A `.dockerignore` file excludes files from the build context before
Docker sends that context to the builder.

For example:

``` dockerignore
node_modules
**/node_modules
.env
```

Why ignore these?

-   `node_modules` can be large; dependencies should be installed in the
    image.
-   `.env` may contain secrets and should not accidentally be copied
    into an image.
-   Excluding unnecessary files makes builds smaller and faster.

The repository's starter `.dockerignore` may only contain
`node_modules`. The pattern `**/node_modules` matches nested
`node_modules` folders too.

Do not copy secrets into an image. Use suitable runtime environment
variables or secret-management mechanisms instead.

## 6. Run a container from the image

After the build succeeds:

``` bash
docker run -d --name todo -p 3000:3000 my-first-build
```

Command breakdown:

  Part               Meaning
  ------------------ -------------------------------------------------------
  `docker run`       Creates and starts a container from an image.
  `-d`               Runs the container in the background (detached mode).
  `--name todo`      Gives the container the name `todo`.
  `-p 3000:3000`     Maps host port 3000 to container port 3000.
  `my-first-build`   The image to use.

### Port mapping

The format is:

``` text
-p HOST_PORT:CONTAINER_PORT
```

For example:

``` bash
docker run -d --name todo -p 8080:3000 my-first-build
```

Then open or call `http://localhost:8080`.

-   `8080` is the port on my computer (host).
-   `3000` is the port inside the container.

Changing only the host port does not require rebuilding the image. The
application must still listen on the container port.

**Remember:** `EXPOSE 3000` does not publish a port. `-p` publishes/maps
it.

## 7. Check the container and its logs

List running containers:

``` bash
docker ps
```

View the application's logs:

``` bash
docker logs todo
```

The official lesson's expected log includes a message similar to:

``` text
Using sqlite database at /etc/todos/todo.db
Listening on port 3000
```

These logs help verify that the application started and which port it is
listening on.

## 8. Test the API

This Dockerfile builds only the backend API. Test the API endpoints
rather than expecting a React page.

### A. Test the greeting endpoint

``` bash
curl http://localhost:3000/api/greeting
```

Expected response:

``` json
{"greeting":"Hello world!"}
```

### B. Create a Todo item

Linux/macOS/WSL or a shell with standard curl:

``` bash
curl -X POST http://localhost:3000/api/items \
  -H "Content-Type: application/json" \
  -d '{"name": "Write a Dockerfile"}'
```

### Windows PowerShell note

In Windows PowerShell, `curl` may be an alias for `Invoke-WebRequest`,
rather than the actual curl program. Use `curl.exe` or PowerShell's
`Invoke-RestMethod`.

A reliable PowerShell option:

``` powershell
$body = '{"name":"Write a Dockerfile"}'

Invoke-RestMethod `
  -Uri "http://localhost:3000/api/items" `
  -Method Post `
  -ContentType "application/json" `
  -Body $body
```

Or use actual curl:

``` powershell
curl.exe -X POST "http://localhost:3000/api/items" -H "Content-Type: application/json" --data-raw '{"name":"Write a Dockerfile"}'
```

### C. Read the Todo items

``` powershell
curl.exe http://localhost:3000/api/items
```

A successful response looks similar to:

``` json
[
  {
    "id": "generated-id",
    "name": "Write a Dockerfile",
    "completed": false
  }
]
```

The actual ID will be different.

**Practice result:** I successfully created a Todo item and retrieved it
from the API running inside my Docker container.

## 9. Why does `http://localhost:3000/` show `Cannot GET /`?

This is expected in this exercise.

The Dockerfile copies only `backend/`. It does not build the React
frontend in `client/`.

The backend serves the frontend from `backend/src/static/`, but that
folder is empty until the React frontend is built and its output is
placed there.

Therefore:

-   `/api/greeting` can work.
-   `/api/items` can work.
-   `/` can return `Cannot GET /`.

This does not mean the container or API is broken. It means this
particular image serves the API only, not the frontend web page.

## 10. RUN vs CMD

These instructions do different jobs:

  -----------------------------------------------------------------------
  `RUN`                               `CMD`
  ----------------------------------- -----------------------------------
  Executes during image build.        Defines the default command for
                                      container startup.

  Its changes are saved in the image. Runs when a container starts.

  Example: `RUN npm ci`               Example:
                                      `CMD ["node", "src/index.js"]`
  -----------------------------------------------------------------------

A simple way to remember:

-   `RUN` = prepare the image.
-   `CMD` = start the application.

## 11. CMD vs ENTRYPOINT

-   `CMD` provides the default command or arguments for a container.
-   `ENTRYPOINT` defines the main executable for the container.
-   A command supplied after the image name in `docker run` normally
    replaces the image's `CMD` (while the entrypoint remains).

Example:

``` bash
docker run --rm my-first-build node --version
```

This runs `node --version` instead of the Dockerfile's normal `CMD`.

Inspect the image's startup configuration:

``` bash
docker image inspect --format '{{json .Config.Entrypoint}} {{json .Config.Cmd}}' my-first-build
```

The `node:22` base image provides an entrypoint script. The Dockerfile
sets the default command to `node src/index.js`.

Use `CMD` for the normal way to start an application. `ENTRYPOINT` is
useful when an image is designed to behave like a particular executable
or CLI tool.

## 12. ENV and USER

### ENV

`ENV` sets an environment variable in the image. It is available to
later build instructions and containers created from the image.

Example:

``` dockerfile
ENV NODE_ENV=production
```

`ENV PORT=3000` is only needed if the application reads
`process.env.PORT` and needs that value supplied. `EXPOSE 3000` does not
set the application's `PORT` variable.

The official six-line Dockerfile for this lesson does not need
`ENV PORT=3000`; the sample backend already listens on port 3000.

Use the syntax:

``` dockerfile
ENV PORT=3000
```

### USER

`USER` sets the user used by later Dockerfile instructions and the
running container.

Containers often run as root by default. Running as a non-root user is a
security best practice, but the user must have permission to access
directories the application needs to write to.

In this sample app, switching to the `node` user without preparing the
SQLite data directory causes a permission error for `/etc/todos`. Do not
add `USER node` blindly; first configure the required file and directory
permissions.

## 13. Clean up

Stop and remove the container:

``` bash
docker rm -f todo
```

-   `docker rm` removes a container.
-   `-f` force-stops it first if it is running.
-   This does not remove the image.

The image can remain available for another run:

``` bash
docker image ls
```

To restore the original repository Dockerfile:

``` bash
git restore Dockerfile
```

## 14. Important concepts to remember

  Concept                 Remember
  ----------------------- --------------------------------------------------------
  Dockerfile              Instructions for building an image.
  Image                   Packaged app, dependencies, and runtime.
  Container               A running (or stopped) instance created from an image.
  Build context           Files/directories Docker can access during a build.
  `.` in `docker build`   Current directory is the build context.
  `-f`                    Selects a Dockerfile path.
  `EXPOSE`                Documents the container port; does not publish it.
  `-p HOST:CONTAINER`     Publishes/maps a container port to the host.
  `RUN`                   Runs at build time.
  `CMD`                   Sets the default startup command.
  `.dockerignore`         Excludes files from the build context.
  `ENV`                   Sets an environment variable.

## 15. Day 02 practical checklist

-   [x] Cloned Docker's Getting Started Todo repository.
-   [x] Identified `client/` as frontend and `backend/` as backend.
-   [x] Understood that this lesson builds only the backend API.
-   [x] Corrected the `COPY` source/destination syntax while practising.
-   [x] Built a Docker image.
-   [x] Ran a container with port mapping.
-   [x] Checked the API using GET and POST requests.
-   [x] Created a Todo item and retrieved it successfully.
-   [x] Understood why `Cannot GET /` is expected.
-   [x] Learned why PowerShell may require `curl.exe` or
    `Invoke-RestMethod`.

## 16. Quick self-test

1.  What is the difference between a Docker image and a container?
2.  What does `WORKDIR` do?
3.  Why does `COPY backend ./` work when the build context is the
    repository root?
4.  What is the difference between `RUN` and `CMD`?
5.  Does `EXPOSE 3000` publish port 3000 to the host?
6.  In `-p 8080:3000`, which port belongs to the host?
7.  Why does `/api/items` work while `/` returns `Cannot GET /`?
8.  Why do we use `.dockerignore`?
9.  Does changing only the host port require rebuilding the image?
10. What does `docker rm -f todo` do?

## 17. Next lesson

**Day 03 --- How image layers and the build cache work**

Next, learn how Docker caches each build instruction, why copying
dependency files before source code can speed up builds, and how to
structure a Dockerfile so a source-code change does not reinstall all
dependencies.

Source: [Docker Docs --- How image layers and the build cache
work](https://docs.docker.com/learn/content/image-layers/)
