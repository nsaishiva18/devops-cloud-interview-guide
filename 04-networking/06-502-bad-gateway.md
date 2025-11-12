## Question  
Your website is returning a **502 Bad Gateway** HTTP status code.  

**Task:**  
Explain what this status code means and list possible root causes and how you'd resolve them.

### 📝 Short Explanation  

A **502 Bad Gateway** error means that a **gateway or proxy server** (like NGINX, HAProxy, or AWS ELB) got an invalid response from the upstream server (like an app server or container).

## ✅ Answer  

---

### 💡 What is 502 Bad Gateway?

A **502** error is returned when the **reverse proxy or load balancer** is **unable to reach the backend service** or gets a **malformed response**.

> Think of it as:  
> "The gatekeeper tried to contact the backend service — but something was wrong or unreachable."

---

### 📘 Common Root Causes

#### 🔌 1. **Backend Service is Down**
- App server (e.g., Node.js, Python, Java) is not running.
- Restart it:
  ```bash
  systemctl status your-app
  ```

---

#### 🔁 2. **Wrong Upstream Configuration in NGINX**
- NGINX is trying to proxy to the wrong port or host.
- Check `nginx.conf` or site config:
  ```nginx
  proxy_pass http://localhost:5000;
  ```

---

#### ⌛ 3. **Backend is Too Slow / Times Out**
- Backend takes too long to respond.
- Adjust timeouts:
  ```nginx
  proxy_read_timeout 60s;
  ```

---

#### ⛔ 4. **Firewall or Security Group Blocking**
- Backend port (e.g., 3000, 5000) is blocked.
- Use `telnet` or `nc` to verify:
  ```bash
  nc -zv localhost 5000
  ```

---

#### 🔒 5. **Incorrect SSL Termination**
- NGINX expects HTTP but backend speaks HTTPS (or vice versa).
- Fix `proxy_pass` protocol (`http://` vs `https://`).

---

#### 🧱 6. **App Crashed or Out of Memory**
- App logs show `OOMKilled`, panic, or crash.
- Check logs:
  ```bash
  journalctl -u your-app
  docker logs your-container
  ```

---

### 🛠️ How to Troubleshoot

1. **Check NGINX error logs**  
   ```bash
   tail -f /var/log/nginx/error.log
   ```

2. **Restart app & NGINX**  
   ```bash
   systemctl restart your-app
   systemctl restart nginx
   ```

3. **Check App Health Endpoint**  
   Test directly with `curl`:
   ```bash
   curl http://localhost:5000/health
   ```

---

### ✅ Bonus: Simulate 502 for Testing

Stop backend app temporarily:
```bash
sudo systemctl stop your-app
```
Then reload the site — you’ll get a 502!

---

> Summary:  
> A **502 Bad Gateway** means your proxy (like NGINX or ELB) could not communicate with your backend service. Check service status, logs, port connectivity, timeouts, and config mismatches to fix it.

---
