## How will you Debug a Live Container?

### Question  
Your application is running inside a Docker container, but it's showing abnormal behavior. How would you debug it without stopping or restarting the container?

### Short explanation of the question  
This tests your ability to troubleshoot and inspect a running container in real time — a critical skill for production debugging and root cause analysis.

### Answer  
I would use `docker exec` or `docker attach` to connect to the running container. Then, I’d inspect logs, processes, file system, and network configuration from within the container.

### Detailed explanation of the answer for readers’ understanding

Docker provides tools to inspect and interact with live containers without restarting or modifying them. Here’s how you do it:

---

### 🛠️ 1. **Use `docker exec` to run commands inside the container**

```bash
docker exec -it <container-id or name> /bin/sh
```

Or for bash-based containers:

```bash
docker exec -it <container-id> /bin/bash
```

This gives you a live shell inside the container, where you can:

- Inspect logs
- Run application commands
- Check environment variables
- Inspect mount points, permissions, file existence

Example:

```bash
docker exec -it myapp cat /var/log/app.log
docker exec -it myapp env
```

---

### 🔌 2. **Use `docker logs` to view container stdout/stderr**

```bash
docker logs -f <container-id>
```

This shows all output from `STDOUT` and `STDERR` of the running container.

Useful for applications that log to console (e.g., Node.js, Python, Java Spring Boot).

---

### 🧾 3. **Use `docker inspect` for metadata and configuration**

```bash
docker inspect <container-id>
```

You can retrieve:

- Mount points
- Network configuration
- Environment variables
- Command and image details

Example:

```bash
docker inspect -f '{{.Config.Env}}' <container-id>
```

---

### 👁️ 4. **Use `docker top` to inspect running processes**

```bash
docker top <container-id>
```

This shows active processes in the container — useful to check if your app is running or stuck.

---

### 📊 5. **Check network settings and connectivity**

```bash
docker exec -it <container-id> netstat -tulnp
docker exec -it <container-id> curl http://localhost:port
```

Useful for diagnosing port binding or DNS resolution issues inside the container.

---

### ⚠️ 6. **Use `docker attach` (with caution)**

```bash
docker attach <container-id>
```

This connects your terminal to the container’s primary process. You can view real-time output and interact with the app.

**Warning**: Pressing `Ctrl+C` may stop the container unless it handles signals properly. Use `Ctrl+P + Ctrl+Q` to detach safely.

---

### 🧠 Real-world Insight

> “During an incident, our containerized Flask app was returning 500s. We `docker exec`’d in, checked the logs, and found a missing environment variable. We patched it by editing the running config and confirmed the fix live before deploying a proper image.”

---

### Key takeaway

> "To debug a live container, use `exec` for shell access, `logs` for output, `inspect` for config, and `top` for processes — all without downtime."

- As it is already live container we can not use 

`docker run -it` - this is used to run the container interactively from scratch.

`docker run -d` - runs container in background.

- So we need to use `docker exec -it <container-id> /bin/bash` or `sh` to get inside the running container.

