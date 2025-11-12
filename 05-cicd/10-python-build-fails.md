## Question  
Python Build Fails on CI But Works Locally — What Can Be the Issue?

### 📝 Short Explanation  
This typically happens due to **differences in environment**, **missing dependencies**, **version mismatches**, or **missing credentials** between your local machine and the CI environment.

## ✅ Answer  

I systematically compare the local and CI environments and address issues related to Python version, packages, permissions, and environment variables.

---

### 🧭 Step-by-Step Troubleshooting

#### 1. 🐍 **Check Python Version**
- Your local machine might use Python 3.11, but the CI runner may default to Python 3.6.
```bash
python --version
```
✅ Fix: Pin the Python version in your CI configuration (e.g., in GitHub Actions):
```yaml
- uses: actions/setup-python@v4
  with:
    python-version: '3.10'
```

---

#### 2. 📦 **Check Dependency Installation**
- Make sure you're installing dependencies from a `requirements.txt` or `pyproject.toml`.
- If CI doesn't install dependencies or installs different ones due to version pinning, the build may fail.

✅ Fix:
```bash
pip install -r requirements.txt
```

---

#### 3. 📁 **Check Missing Files/Modules**
- CI systems start from a clean environment. Ensure all required files (e.g., `.env`, `config.yaml`, local modules) are **checked into Git** or provided securely.

---

#### 4. 🔐 **Check for Missing Secrets**
- Locally, your credentials (e.g., AWS, database) might be stored in `.env`, but CI needs them as **environment variables** or **secret manager injections**.

✅ Fix in GitHub Actions:
```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

---

#### 5. 🧪 **Run Tests Verbosely**
- In CI, run tests with verbose output:
```bash
pytest -v
```
- It helps pinpoint the exact failure, such as ImportError, ModuleNotFound, or permission issues.

---

#### 6. 📦 **Check for C Extension Issues**
- Some Python packages require system-level dependencies (e.g., `psycopg2`, `lxml`).
- These might be installed on your machine, but not available in the CI container.

✅ Fix: Add dependencies via `apt` before pip install:
```bash
sudo apt-get install -y libpq-dev
pip install psycopg2
```

---

### 🧠 Real-World Example

A Flask app was building fine locally but failed in GitHub Actions CI.  
Reason: The app used `python-dotenv`, but `.env` file was not available in CI, and environment variables were not set.

✅ Fix:
- Added secrets in GitHub → Repository Settings → Secrets
- Injected them in the pipeline using `env:`

---

> Summary:  
> When a Python build fails in CI but works locally,

> - Will check logs of the CI -- Timeout, lack of resources.
> - Check version differences  
> - Validate dependency installation(If developer is using virtual env or not)
> - Ensure all required files and env vars are available
> - Review system-level dependencies and module paths
> - Caching issues - when developer is running locally, build command is picking earlier version from artifact.

---
