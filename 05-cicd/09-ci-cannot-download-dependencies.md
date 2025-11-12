## Question  
Your build fails because it can’t download a dependency from your artifact repository. What will you do?

### 📝 Short Explanation  
A failed dependency download usually indicates an issue with **repository configuration**, **authentication**, **connectivity**, or **artifact availability**.

## ✅ Answer  

When a build fails to fetch a dependency from an artifact repo (e.g., Artifactory, Nexus, AWS CodeArtifact), I debug it systematically.

---

### 🧭 Step-by-Step Troubleshooting

#### 1. 🔍 **Check the Build Error Message**
- Identify the exact dependency and which repo URL it tried to hit.
- Note if it’s a `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, or a timeout.

---

#### 2. 🗂️ **Verify Repository Configuration**
- Check your `pom.xml`, `build.gradle`, or `.npmrc` to ensure the **repository URL is correct**.

✅ For Maven:
```xml
<repository>
  <id>company-repo</id>
  <url>https://artifactory.example.com/artifactory/libs-release</url>
</repository>
```

---

#### 3. 🔐 **Check Credentials**
- Verify credentials in `~/.m2/settings.xml` or environment variables are correct and not expired.

✅ Sample settings.xml:
```xml
<server>
  <id>company-repo</id>
  <username>${env.ARTIFACT_USER}</username>
  <password>${env.ARTIFACT_PASS}</password>
</server>
```

> Tip: Some tokens (e.g., AWS CodeArtifact) expire and need to be refreshed via CLI.

---

#### 4. 🌐 **Test Repo Connectivity**
- Manually curl the artifact URL to check if it is reachable:
```bash
curl -I https://artifactory.example.com/artifactory/libs-release/...
```

- Check for proxy/firewall/DNS issues especially in CI environments.

---

#### 5. 📦 **Check If the Artifact Exists**
- Log into the artifact repository UI and confirm:
  - The artifact version exists
  - The repository hasn’t been deleted or archived

---

#### 6. 🛠️ **Try a Local Build with Clean Cache**
- Delete local repo cache and try again:
```bash
rm -rf ~/.m2/repository/<group>/<artifact>
mvn clean install
```

---

### 🧠 Real-World Example

We once faced this with `mvn install` failing in Jenkins.  
Reason: The **CodeArtifact token had expired**, but Jenkins was still using the old token via environment variable.

✅ Fix:  

- Added a step in Jenkinsfile to refresh the token before the build:

```bash
aws codeartifact get-authorization-token ...
```

---

> Summary:  
> When a build fails due to missing dependencies, I:

> - Check error logs and repo URL
> - Validate credentials and token freshness
> - Confirm repo and artifact existence
> - Test connectivity manually
> - Clean local cache and retry

---
