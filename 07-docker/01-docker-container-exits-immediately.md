## Docker Container Exits Immediately — Troubleshooting

### Question  
Docker container exits immediately, how will you troubleshoot?

### Short explanation of the question  
This is a common scenario where a container runs and stops right away without staying alive. The interviewer wants to assess your debugging skills, understanding of container lifecycle, and awareness of entrypoint behavior.

### Answer  
I would first inspect the container logs, verify the Dockerfile entrypoint or command, and check if the container runs a long-lived process. Often, containers exit if the main process completes or crashes.

### Detailed explanation of the answer for readers’ understanding

When a Docker container starts, it runs the command defined in `CMD` or `ENTRYPOINT`. If that process ends — whether successfully or due to an error — the container will exit.

---

### 🧪 Step-by-step Troubleshooting

#### 1. **Check logs of the container**
```bash
docker logs <container_id_or_name>
```
Look for error messages, exceptions, or early termination messages from the main process.

#### 2. **Inspect the Dockerfile (if accessible)**
See what `CMD` or `ENTRYPOINT` is being run. For example:

```Dockerfile
CMD ["python", "app.py"]
```

If `app.py` exits immediately, the container will too.

#### 3. **Run container in interactive mode for debugging**
```bash
docker run -it <image> /bin/bash
```
This helps you step into the container and debug further (check logs, dependencies, paths, etc.).

#### 4. **Override CMD/ENTRYPOINT temporarily**
You can override the command during runtime to run a shell:
```bash
docker run -it <image> /bin/sh
```

#### 5. **Check the exit status**
```bash
docker inspect <container_id> --format='{{.State.ExitCode}}'
```
A non-zero exit code often indicates an error.

---

### 🧠 Real-world Example

> "I faced this when deploying a Node.js app — the Dockerfile’s `CMD` was running `node index.js`, but the file wasn’t copied properly during the build. The logs showed 'cannot find module'. I fixed it by checking the `COPY` command in Dockerfile."

---

### Key takeaway

> "A Docker container must run a long-running foreground process. If it finishes or crashes, the container exits. Use `docker logs`, `-it` mode, and inspect `CMD/ENTRYPOINT` to debug such issues effectively."

```
Docker containers are meant to be a single process containers.
- If the command or process specified in the container completes quickly, or may be if it falls immediately due to an error, or if it is not a long-running process, the container will exit immediately.
```