## Question  
CI Pipeline Succeeds but App is Broken in Prod — What Action Will You Take?

### 📝 Short Explanation  
If the CI/CD pipeline passes but the application breaks in production, it suggests that something is **missing in the validation phase**, or there are **environment mismatches** between staging/CI and production.

## ✅ Answer  

I treat this as a **critical incident** and approach it using a mix of **debugging**, **rollbacks**, and **preventive actions** for the future.

---

### 🔍 Step-by-Step Troubleshooting Approach

#### 1. 🔥 **Initiate Incident Response**
- Notify the team. Document the issue.
- If user-facing and severe, consider triggering an **automated rollback** or **manual redeploy of the last working version**.

#### 2. 🧪 **Check Prod Logs and Monitoring**
- View logs using ELK, CloudWatch, or your observability stack.
- Check:
  - HTTP status codes (500s, 4xx)
  - Application logs
  - Metrics: memory, CPU, DB errors, timeouts

#### 3. 🔁 **Compare Staging and Production**
- Is staging missing any:
  - Environment variables?
  - Backend services?
  - Feature flags?
- Confirm the **artifact promoted to prod** is the same tested in staging.

#### 4. 🔐 **Check Secrets and External Integrations**
- API keys, third-party integrations, database credentials — misconfigured or rotated tokens can break production unexpectedly.

#### 5. 🧾 **Check Infrastructure Differences**
- Is production using a different:
  - Kubernetes namespace?
  - Load balancer config?
  - Terraform state?
- Sometimes prod has older AMIs, different volumes, or a custom security group.

---

### ✅ Immediate Actions

- **Rollback if feasible** (using Git tags, Helm chart versions, AMI snapshots).
- **Create a postmortem** entry and assign root cause analysis (RCA).
- **Patch with hotfix** only after RCA is complete.

---

### 🛠️ Preventive Measures Going Forward

- Add **automated smoke tests** after deployment.
- Integrate **canary releases** or **blue-green deployments**.
- Enforce staging and production **parity** in environments.
- Validate secrets and config before each deployment.
- Enable alerting for anomalies immediately after deployment.

---

### 🧠 Real-World Example

After a successful CI build and deployment, the app was broken in prod because a staging-only environment variable (`USE_MOCK_SERVICE=true`) was hardcoded and not overridden in production.

🔧 Fix: Added proper `values-prod.yaml` for Helm, separated environments clearly, and added smoke tests post-deploy.

---

> Summary:  
> A CI pipeline success doesn’t always guarantee production stability. I validate logs, environment parity, secrets, and roll back if needed. Going forward, I strengthen post-deploy checks and staging fidelity.

---


- Traffic flow for prod and stage may vary.
- Will check logs --> Crashloop backoff.