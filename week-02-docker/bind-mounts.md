# Docker Bind Mounts

## 1. What is a Bind Mount?

A Bind Mount connects an existing folder or file on the host machine to a folder inside a Docker container.

Both locations access the same files. Changes are visible in both places.

**Example:**

- Host: `~/docker-practice`
- Container: `/app`

## 2. How Bind Mounts Work

```text
Host Machine                 Docker Container
~/docker-practice    <---->  /app
```

- Create a file on the host → Available inside the container.
- Modify a file inside the container → Updated on the host.
- Modify a file on the host → Updated inside the container.

**Important:** A Bind Mount does not create a separate copy of the files. Both locations access the same underlying data.

## 3. Create a Bind Mount Using `-v`

### Create a practice folder

```bash
mkdir -p ~/docker-practice
cd ~/docker-practice
```

### Create a file

```bash
echo "Hello from my computer!" > host.txt
```

### Run a container

```bash
docker run -it --name bind-test -v ~/docker-practice:/app ubuntu bash
```

### Check the mounted folder

```bash
ls /app
cat /app/host.txt
```

The host file is accessible inside the container.

### Modify the file inside the container

```bash
echo "Hello from Docker container!" >> /app/host.txt
```

Exit the container:

```bash
exit
```

Check the file on the host:

```bash
cat ~/docker-practice/host.txt
```

The new text is also available on the host.

## 4. Two-Way Synchronization

Changes are visible in both directions.

**Host to Container:**

```bash
echo "Updated from host!" >> ~/docker-practice/host.txt
```

**Container to Host:**

```bash
echo "Updated from container!" >> /app/host.txt
```

Both locations access the same file.

## 5. Using `--mount`

Docker also supports another syntax for Bind Mounts.

```bash
docker run -it --name bind-test-2 \
  --mount type=bind,source=/home/USERNAME/docker-practice,target=/app \
  ubuntu bash
```

Replace `USERNAME` with your actual WSL username.

### Understanding the syntax

- `type=bind` — Specifies a Bind Mount.
- `source` — Path of the host folder.
- `target` — Destination path inside the container.

## 6. `-v` vs `--mount`

| `-v` | `--mount` |
|---|---|
| Short syntax | Descriptive syntax |
| `host:container` | `source=...,target=...` |
| Easier to write | Easier to understand |
| Can create a missing host directory automatically | Missing source directory causes an error by default |

Both methods can create the same Bind Mount.

## 7. Important Docker Commands

| Command | Purpose |
|---|---|
| `docker run` | Creates and starts a new container |
| `docker start` | Starts an existing stopped container |
| `docker exec -it CONTAINER bash` | Opens an interactive Bash terminal in a running container |
| `docker stop` | Stops a running container |
| `docker ps` | Lists running containers |
| `docker ps -a` | Lists all containers |

### Important Flags

| Flag | Meaning |
|---|---|
| `-v` | Mounts a folder or volume |
| `--mount` | Uses descriptive mount syntax |
| `-d` | Runs a container in detached mode |
| `-it` | Interactive terminal |
| `--name` | Assigns a container name |

## 8. When to Use Bind Mounts?

**Development:**
- React, Node.js and other application development.
- Edit code on the host using VS Code.
- Container accesses updated code without rebuilding the image.

**Configuration:**
- Share configuration files with containers.
- Mount local files into containers.

## 9. Bind Mount vs Docker Volume

| Bind Mount | Docker Volume |
|---|---|
| Uses an existing host path | Managed by Docker |
| You choose the host directory | Docker manages storage location |
| Useful for application development | Useful for persistent application data |
| Easy to access and edit host files | Better managed by Docker |

### Remember

- Bind Mount = Connect an existing host folder to a container.
- Volume = Docker-managed persistent storage.
- Development code = Usually Bind Mount locally.
- Database data = Usually Docker Volume.
- Production application code = Usually included in the Docker image.

**Key takeaway:** Bind Mounts are especially useful when you want to edit files on your computer while a Docker container uses those same files.