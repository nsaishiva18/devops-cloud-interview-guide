## Question  
Pipeline Slows Down Over Time (Builds taking more time) — How Will You Fix?

### 📝 Short Explanation  
If a CI pipeline is progressively getting slower, it's likely due to **accumulated build artifacts**, **unoptimized steps**, **lack of caching**, or **resource saturation** on the runner/agent.

## ✅ Answer  

When I notice that builds are getting slower over time, I take a **metrics-driven approach** to isolate the slowdown and optimize the pipeline stages.

---

### 🧭 Step-by-Step Troubleshooting Approach

#### 1. 📊 **Measure Stage Durations Over Time**
- Use CI tool metrics (Jenkins, GitHub Actions, GitLab) or integrate Prometheus/Grafana.
- Identify **which stage(s)** are consuming more time — code checkout, dependency resolution, test execution, build packaging, etc.

---

#### 2. 📦 **Check Dependency Management**
- Over time, dependency trees can grow.
- Use tools like:
  - `mvn dependency:analyze`
  - `npm prune`
- Cache dependencies (e.g., `~/.m2`, `node_modules`) between runs to avoid full re-downloads.

---

#### 3. 💾 **Enable Layered and Incremental Builds**
- Avoid cleaning entire workspace unless necessary (`mvn clean install` can be expensive).
- Use incremental build options:
  - Gradle: `--build-cache`
  - Bazel: native caching
  - Docker: use layers wisely and avoid invalidating cache

---

#### 4. 🧼 **Clean Up Disk and Workspace**
- Runners may accumulate:
  - Gigabytes of build artifacts
  - Old Docker images and volumes
- Use scheduled jobs or add cleanup logic:
  ```bash
  docker system prune -af
  ```

---

#### 5. 🚀 **Use Parallelism and Matrix Builds**
- Split long test suites or build steps using:
  - `strategy.matrix` in GitHub Actions
  - `parallel` stages in Jenkins pipelines

---

#### 6. ⚙️ **Review Runner/Agent Resource Utilization**
- Check CPU, memory, disk I/O on build agents.
- Use autoscaling runners or move to faster instance types if needed.

---

### 🧠 Real-World Example

We noticed our build time increased from 7 to 19 minutes over 6 months.  
Root cause:  

- Increased number of tests without parallel execution  
- Outdated Docker layers not using cache  

✅ Fixes implemented:

- Introduced parallel test stages  
- Refactored Dockerfile to leverage cache better  
- Cleaned up unused images on runners weekly  

---

> Summary:  
> When a pipeline slows down, I:

> - Measure and isolate the slowdown  
> - Optimize dependencies and builds  
> - Use caching and parallelism  
> - Clean up workspace and review resource usage

---
