Here is a concise, organized guide to the most essential Docker commands for your notes.

---

## Essential Docker Commands Reference

### 1. General & Information

Commands to check the status of your Docker environment.
- `docker version` – Shows the installed version of Docker and its components.
- `docker info` – Displays system-wide information (containers, images, storage driver, etc.).
- `docker help [command]` – Displays help for a specific command.
---
### 2. Working with Images

Images are the "blueprints" for your containers.

| **Command**                      | **Description**                                                 |
| -------------------------------- | --------------------------------------------------------------- |
| `docker pull <image>`            | Downloads an image from Docker Hub.                             |
| `docker images`                  | Lists all locally stored images.                                |
| `docker build -t <name>:<tag> .` | Builds an image from a **Dockerfile** in the current directory. |
| `docker rmi <image_id>`          | Removes a specific image.                                       |
| `docker image prune`             | Removes all unused images.                                      |

---

### 3. Container Management

Commands to create, start, and stop containers.

| Command                                              | Description                                         |
| :--------------------------------------------------- | :-------------------------------------------------- |
| `docker run -d --name <name> <image>`                | Runs a container in **detached mode** (background). |
| `docker run -p <host_port>:<container_port> <image>` | Maps a port from your machine to the container.     |
| `docker ps`                                          | Lists all **running** containers.                   |
| `docker ps -a`                                       | Lists **all** containers (including stopped ones).  |
| `docker stop <container_id>`                         | Gracefully stops a running container.               |
| `docker start <container_id>`                        | Starts a stopped container.                         |
| `docker rm <container_id>`                           | Deletes a stopped container.                        |

---

### 4. Interacting with Containers

Use these to see what is happening inside your applications.

| Command | Description |
| :--- | :--- |
| `docker logs <container_id>` | Fetches the logs of a container. |
| `docker logs -f <container_id>` | Follows the log output in real-time. |
| `docker exec -it <container_id> /bin/bash` | Opens an **interactive terminal** inside a running container. |
| `docker top <container_id>` | Displays the running processes of a container. |

---

### 5. Cleanup & Maintenance

Docker can consume a lot of disk space over time. These commands help you reclaim it.

| Strategy | Command | Description |
| :--- | :--- | :--- |
| **The "Nuclear" Option** | `docker system prune` | Removes all stopped containers, unused networks, and dangling images. |
| **Deep Clean** | `docker system prune -a --volumes` | Removes everything not currently in use, including volumes and all unused images. |

---

### 6. Docker Compose

For managing multi-container applications (usually defined in `docker-compose.yml`).

| Command | Description |
| :--- | :--- |
| `docker-compose up -d` | Starts all services defined in the file in the background. |
| `docker-compose down` | Stops and removes all containers, networks, and images defined in the file. |
| `docker-compose logs -f <service_name>` | View logs for a specific service or all services. |
| `docker compose config` | View the final configuration and `.env` variable mapping. |
### 7. Interacting with private repo
```bash
docker login <nexus-registry-url>:<port>
docker tag <local-image-name>:<tag> <nexus-registry-url>:<port>/<repository-name>/<image-name>:<tag>

docker push <nexus-registry-url>:<port>/<repository-name>/<image-name>:<tag>
```