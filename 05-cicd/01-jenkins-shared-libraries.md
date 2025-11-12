## Question  
What are Jenkins Shared Libraries and how do they work?

### 📝 Short Explanation  
Jenkins Shared Libraries allow you to **centralize reusable pipeline code** (like functions, steps, and variables) across multiple pipelines. They promote **code reuse**, **maintainability**, and **consistency** in large Jenkins setups.

## ✅ Answer  

Below are common for 
- Static code analysis
- Build Stage is same as we mostly have java applications
- Deployment logic is same


---

### 📘 What is a Jenkins Shared Library?

A **Shared Library** is a Git repository (or part of one) that contains reusable Groovy code you can include in Jenkins pipelines using the `@Library` annotation.

It typically includes:
```
(root)
├── vars/
│   └── sayHello.groovy
├── src/
│   └── org/example/MyClass.groovy
├── resources/
│   └── templates/config.xml
└── README.md
```

---

### 🧠 Why Use Shared Libraries?

- Avoid repeating logic in every Jenkinsfile  
- Encapsulate business logic, deployment steps, or validation code  
- Easy updates across all pipelines  
- Fewer errors and better collaboration  

---

### ⚙️ How Do They Work?

1. **Create a Git repo** with a specific structure (`vars/`, `src/`, etc.).
2. Configure the library in Jenkins:
   - Go to **Manage Jenkins → Global Pipeline Libraries**.
   - Add your library by name and Git URL.
3. In your Jenkinsfile, you load the library:
   ```groovy
   @Library('my-shared-library') _
   ```
4. Use global functions or classes defined in `vars/` or `src/`.

---

### 🔍 Example

**vars/sayHello.groovy**
```groovy
def call(String name = 'world') {
    echo "Hello, ${name}!"
}
```

**Jenkinsfile**
```groovy
@Library('my-shared-library') _

pipeline {
    agent any
    stages {
        stage('Greet') {
            steps {
                sayHello('Abhishek')
            }
        }
    }
}
```

---

### ✅ Benefits

- DRY (Don’t Repeat Yourself)
- Cleaner Jenkinsfiles
- Version-controlled and auditable
- Easier team collaboration

---

> Summary:  
> Jenkins Shared Libraries allow you to **modularize and reuse pipeline logic** across projects. They are ideal for **large-scale CI/CD environments** where consistency and maintainability are key.

---
