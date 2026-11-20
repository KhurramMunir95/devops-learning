# Docker Volumes

## 1. What are Docker Volumes?

Docker Volumes are used to store data permanently, even if a container is deleted.

**Purpose:** To keep important data safe outside the container.

## 2. Why do we need Volumes?

Without volumes:
- Data is stored in the container's writable layer.
- If the container is removed, its data is lost.

With volumes:
- Data is stored separately from the container.
- If the container is removed, volume data remains safe.

**Note:** Stopping or restarting a container does not delete its data.

## 3. How Volumes Work

Example:

```bash
-v mongo-data:/data/db
```

- `mongo-data` = Docker Volume name.
- `/data/db` = MongoDB's default data directory inside the container.
- `-v` = Mounts the volume to the container.

Docker mounts the volume at `/data/db`, and MongoDB reads and writes data through that path.

**Remember:**
- Actual persistent data is stored in the volume.
- MongoDB knows `/data/db`, but it doesn't need to know the Docker volume name.
- Docker manages the volume storage.

## 4. Without Volume vs With Volume

| Without Volume | With Volume |
|---|---|
| Data stored in container's writable layer | Data stored in volume |
| Removing container deletes its writable data | Removing container keeps volume data |
| Temporary storage | Persistent storage |

## 5. Practical: Create a Volume

### Step 1: Create Volume

```bash
docker volume create mongo-data
```

Creates a volume named `mongo-data`.

### Step 2: List Volumes

```bash
docker volume ls
```

Shows all Docker volumes.

### Step 3: Run MongoDB Container

```bash
docker run -d \
  --name mongodb \
  -p 27017:27017 \
  -v mongo-data:/data/db \
  mongo
```

**Command explanation:**

| Command / Flag | Purpose |
|---|---|
| `docker run` | Creates and starts a container |
| `-d` | Runs in background |
| `--name` | Sets container name |
| `-p` | Maps host and container ports |
| `-v` | Mounts a volume |
| `mongo` | Docker image name |

### Step 4: Access MongoDB Shell

```bash
docker exec -it mongodb mongosh
```

- `exec` = Runs a command inside a running container.
- `-i` = Keeps input open.
- `-t` = Provides a terminal interface.
- `mongosh` = Opens MongoDB shell.

Insert sample data:

```javascript
use volume_test

db.users.insertOne({
  name: "Test User",
  role: "Developer"
})

db.users.find()
```

### Step 5: Remove Container

```bash
docker rm -f mongodb
```

- `rm` = Removes container.
- `-f` = Forcefully removes it, even if running.

### Step 6: Create Container Again

Run the same MongoDB command again:

```bash
docker run -d \
  --name mongodb \
  -p 27017:27017 \
  -v mongo-data:/data/db \
  mongo
```

### Step 7: Verify Data

```bash
docker exec -it mongodb mongosh
```

Then:

```javascript
use volume_test
db.users.find()
```

**Result:** Previously inserted data is still available because it was stored in the Docker volume.

## 6. Useful Volume Commands

| Command | Purpose |
|---|---|
| `docker volume ls` | Lists all volumes |
| `docker volume create` | Creates a volume |
| `docker volume inspect` | Shows volume details |
| `docker volume rm` | Removes a volume |
| `docker volume prune` | Removes unused volumes |

## 7. Check Volume Location

```bash
docker volume inspect mongo-data
```

On Linux, Docker's default volume location is:

```text
/var/lib/docker/volumes/
```

Docker Desktop with WSL2 manages its storage inside its Linux environment.

## 8. Connect MongoDB Compass

Connection string:

```text
mongodb://localhost:27017
```

- Connect MongoDB Compass to the running Docker container.
- Open `volume_test` database.
- Open `users` collection.
- View inserted documents.

## Key Takeaways

- Container = Runs the application.
- Image = Blueprint used to create a container.
- Volume = Persistent data storage.
- Mount = Attaches storage to a specific container path.
- MongoDB uses `/data/db` as its data directory.
- Docker manages the volume behind that path.
- Container can be deleted without losing volume data.

**Remember:** Container can be temporary, but Volume keeps data persistent until the volume itself is deleted.