## Question  
Build Passed Locally but Fails in CI — How Will You Troubleshoot?

### 📝 Short Explanation  
A common CI/CD issue is when the build works on a developer’s machine but fails in the CI environment. This usually points to **differences in configuration, environment, or dependencies**.

## ✅ Answer  

I troubleshoot this in a structured way, focusing on **environmental differences** and **repeatability**.

---

### 🧭 Step-by-Step Troubleshooting Approach

#### 1. ✅ **Check the Error Log in CI**
- Start with the **exact error message** in the CI logs.
- Identify if it’s a **compilation error**, **test failure**, **dependency issue**, or **permission problem**.

#### 2. 🧪 **Reproduce Locally in CI-Like Environment**
- Use a clean Docker image or build the app on a new VM.
- Mimic the CI build environment as closely as possible (e.g., same JDK, Node, Python version).

```bash
docker run -it --rm openjdk:17 bash
# Then run your build commands
```

#### 3. 🔁 **Compare Local and CI Environments**
- Check:
  - OS version
  - Language/toolchain versions
  - Environment variables
  - Build tools (Maven/Gradle versions)
  - Memory/CPU limits

#### 4. 🔐 **Verify Secrets and Tokens**
- Builds often fail in CI due to missing:
  - Git credentials
  - API tokens
  - `.npmrc` or `.pypirc` config files

Ensure secrets are properly injected via CI environment variables or secrets management tools.

#### 5. 📦 **Dependency Issues**
- Check if the build pulls dependencies from a local cache (e.g., `~/.m2/repository`) that CI doesn’t have.
- Add a `mvn dependency:tree` or `npm ls` check to compare.

#### 6. 📁 **File/Path Issues**
- Case sensitivity, permissions, and missing files often break builds in Linux-based CI runners.

---

### 🧠 Example

The build passed locally but failed in CI with:
```
error: package javax.annotation does not exist
```

🔍 Root Cause: Local machine had Java 8. CI was using Java 11. The missing package wasn’t included in Java 11 by default.

✅ Fix: Added `javax.annotation` as an explicit dependency in `pom.xml` and pinned the JDK version in CI to match local.

---

> Summary:  
> When a build passes locally but fails in CI, I:

> - Start with CI logs,
> - Will check if developer is using `mvn clean` before `mvn install` or `mvn package` to ensure no stale artifacts,
> - Reproduce the issue in a clean container,
> - Compare environments (Will compare with developer local environment and CI)
> - Missing file permissions,
> - Check for missing dependencies or secrets,
> - Fix and make the build reproducible and environment-agnostic.

---
