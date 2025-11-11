## Question  
Linux Server is slow due to high CPU utilization. How will you fix it?

### 📝 Short Explanation  
This question assesses your ability to diagnose performance issues, identify root causes, and take targeted actions to reduce CPU load on a production server.

## ✅ Answer  
I would begin by identifying which processes are consuming the most CPU using tools like `top`, `htop`, or `pidstat`, then analyze whether it's due to a misbehaving application, runaway process, or scheduled job. Based on the findings, I’d take corrective action — either by killing the process, adjusting resource limits, or scaling the workload.

```sh
# Find out if any process is running with more CPU and try to kill if that is not imp if imp then decrease the priority.

nice or renice pid 

```

### 📘 Detailed Explanation  

---

### ✅ Step 1: Check Load Average  
```bash
uptime
```
Example output:
```
14:02:03 up  3 days,  4:55,  2 users,  load average: 6.02, 4.33, 2.89
```
A load average consistently higher than the number of CPU cores indicates overutilization.

---

### ✅ Step 2: Identify CPU-Heavy Processes  
```bash
top -o %CPU
```
or more interactively:
```bash
htop
```

This shows which processes are consuming the most CPU.

---

### ✅ Step 3: Drill Down with `ps` or `pidstat`  
```bash
ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu | head
```

or:
```bash
pidstat -u 1 5
```

These give detailed insight into CPU consumption over time.

---

### ✅ Step 4: Investigate the Cause  
Based on what you see, ask:
- Is it a specific app (e.g., Java, Python, Node.js)?
- Is there a cron job or batch script running?
- Is a service misconfigured and looping?
- Is it caused by a known bug (e.g., zombie processes)?

---

### ✅ Step 5: Take Corrective Action  
- Kill or restart runaway process:
  ```bash
  kill -9 <pid>
  systemctl restart <service>
  ```
- Scale the application or move workloads
- Limit resource usage using `nice`, `cpulimit`, or cgroups
- Tune app performance (e.g., DB queries, memory leaks)

---

### ✅ Step 6: Check Logs  
```bash
journalctl -xe
tail -f /var/log/syslog
```
Logs may reveal:
- App crashes
- High retry loops
- Configuration issues

---

### ✅ Step 7: Implement Preventive Measures  
- Set CPU/memory limits in containerized apps
- Use monitoring tools like `Prometheus + Grafana`
- Configure alerts for high CPU (e.g., above 80% for 5 mins)
- Refactor long-running or expensive tasks

---

### 🧠 Real-Life Examples:
- A cron script looping due to a bad condition
- A Java app stuck in infinite recursion
- Docker containers running unbounded scraping jobs
- Antivirus or audit daemon consuming CPU after log floods

> Summary:  
> Use `top`, `htop`, `ps`, and `pidstat` to identify heavy processes. Fix the root cause and add monitoring to avoid similar issues in the future.

---
