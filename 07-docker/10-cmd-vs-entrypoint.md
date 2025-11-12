## Explain the difference between CMD and ENTRYPOINT in Docker

### Question  
What is the difference between `CMD` and `ENTRYPOINT` in a Dockerfile?

### Short explanation of the question  
This question evaluates your understanding of how Docker containers are initialized — specifically, how default commands are defined and how they behave when overridden at runtime.

### Answer  
Both `CMD` and `ENTRYPOINT` define what runs when a container starts, but:

- `ENTRYPOINT` is fixed and always executed. Can not override the default arguments.

- `CMD` provides default arguments that can be overridden.

When combined, `CMD` acts as arguments to the `ENTRYPOINT`.

---

### Detailed explanation of the answer for readers’ understanding

#### 🔸 CMD

- Defines **default command** and/or arguments to run inside the container.
- Can be **overridden** at runtime by providing a new command.

```Dockerfile
FROM ubuntu
CMD ["echo", "Hello from CMD"]
```

Running this container:

```bash
docker run myimage
# Output: Hello from CMD
```

Override:

```bash
docker run myimage echo "Overridden"
# Output: Overridden
```

---

#### 🔸 ENTRYPOINT

- Defines a **fixed command** that always runs.
- Arguments can be passed at runtime, but the command remains the same.

```Dockerfile
FROM ubuntu
ENTRYPOINT ["echo"]
```

Running the container:

```bash
docker run myimage Hello from ENTRYPOINT
# Output: Hello from ENTRYPOINT
```

You can't override `ENTRYPOINT` without using `--entrypoint` flag.

---

### 🔄 Using ENTRYPOINT + CMD Together

```Dockerfile
FROM ubuntu
ENTRYPOINT ["echo"]
CMD ["Hello from CMD"]
```

Running the container:

```bash
docker run myimage
# Output: Hello from CMD
```

Or with arguments:

```bash
docker run myimage Custom message
# Output: Custom message
```

Here:
- `ENTRYPOINT` = `echo`
- `CMD` = default args → `Hello from CMD`

So final command = `echo Hello from CMD`

---

### 🧠 Real-world Example

> “In one project, our Docker image used `ENTRYPOINT` to run the app’s binary. We passed different flags for `staging` and `prod` via `CMD`, which worked well across environments without changing the image.”

---

### 📝 Summary Table

| Feature        | CMD                          | ENTRYPOINT                    |
|----------------|-------------------------------|--------------------------------|
| Purpose        | Default command or args       | Fixed executable              |
| Overridable    | ✅ Easily overridden           | ❌ Only with `--entrypoint`   |
| Combination    | Acts as args to ENTRYPOINT    | Works with CMD as default args|

---

### Key takeaway

> "Use ENTRYPOINT to define the main command, and CMD to define default arguments. This gives flexibility to override while keeping a consistent entry behavior."

- When it comes to `ENTRYPOINT` and `CMD`. When we run the container, these are the first commands to get executed.

- Where as when it comes to CMD - you can override the default parameters by passing argument to the docker run command.

- Where as with ENTRYPOINT you can not do it.

Q. Can you override through ENTRYPOINT as well ?

A. Yes, in Docker run command we need to pass --entrypoint.

