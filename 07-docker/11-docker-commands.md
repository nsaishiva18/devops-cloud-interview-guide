## What Docker commands do you use on a day-to-day basis?

### Question  
What are the most common Docker commands you use regularly while working with containers?

### Short explanation of the question  
This question evaluates your practical experience with Docker — whether you're just running containers or actively building, debugging, and cleaning up Docker environments.

### Answer  
Some of the Docker commands I use regularly include `docker ps`, `docker build`, `docker run`, `docker logs`, `docker exec`, and `docker system prune`. These help in building images, starting/stopping containers, debugging logs, and maintaining disk space.

---

### Detailed explanation of the answer for readers’ understanding

Here are **10 Docker commands** that I use frequently, with a short explanation and example for each:

---

### 1. `docker ps`

Shows running containers.

```bash
docker ps
```

Add `-a` to see all containers (including exited ones).

---

### 2. `docker build`

Builds a Docker image from a Dockerfile.

```bash
docker build -t myapp:latest .
```

---

### 3. `docker run`

Runs a container from an image.

```bash
docker run -d -p 8080:80 myapp
```

---

### 4. `docker exec`

Execute a command inside a running container.

```bash
docker exec -it myapp_container /bin/bash
```

Great for debugging inside the container.

---

### 5. `docker logs`

View logs from a running container.

```bash
docker logs -f myapp_container
```

Use `-f` to follow log output in real-time.

---

### 6. `docker stop` and `docker start`

Stop or restart a running container.

```bash
docker stop myapp_container
docker start myapp_container
```

---

### 7. `docker images`

Lists all images on your system.

```bash
docker images
```

Use it to check image size, version, etc.

---

### 8. `docker rm` and `docker rmi`

Remove a container or image.

```bash
docker rm myapp_container
docker rmi myapp:latest
```

---

### 9. `docker system prune`

Cleans up unused containers, images, and networks to free space.

```bash
docker system prune -a
```

---

### 10. `docker inspect`

Inspect metadata about containers and images.

```bash
docker inspect myapp_container
```

Useful for debugging network, mount paths, environment variables, etc.

---

### 🧠 Real-world Insight

> “While troubleshooting container issues, I often use `docker exec` to get a shell, check logs with `docker logs`, and restart the container if needed. During build optimizations, `docker system prune` helped free up over 5GB of unused image data.”

---

### Key takeaway

> "Mastering core Docker commands like `build`, `run`, `exec`, and `logs` makes daily container work efficient and helps troubleshoot quickly."


```
docker build - used to build docker image from docker file.

docker run - once the image is build, run is used to run the container.

docker ps  -

docker images -
docker logs  -
docker rmi - 



```