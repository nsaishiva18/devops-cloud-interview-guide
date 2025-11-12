## Question  
Using Static Code Analysis, what kind of problems can you identify?

### 📝 Short Explanation  
Static code analysis helps detect issues in the source code **without executing it**. It's an early gate in the CI/CD pipeline to catch bugs, code smells, and violations of coding standards.

## ✅ Answer  

Static code analysis tools can identify a wide range of issues, including:

---

### 🔍 Types of Problems Identified by Static Analysis

#### 1. 🧠 **Syntax Errors and Language Misuse**
- Invalid syntax or misuse of language constructs.
- Missing imports, undeclared variables, etc.
```python
def test():
    print(x)   # x not defined
```

#### 2. 📏 **Coding Standard Violations**
- Violations of PEP8 in Python, PSR-12 in PHP, or Google's Java Style Guide.
- Examples:
  - Too long lines
  - Improper indentation
  - Poor variable naming

Tools: `pylint`, `flake8`, `checkstyle`, `eslint`

---

#### 3. 🔁 **Code Complexity and Maintainability**
- Detects overly complex functions or nested logic.
- Warns about:
  - Deep nesting
  - Too many return points
  - Long methods or files

Tool: `radon`, `sonarqube`, `jshint`

---

#### 4. 🔐 **Security Vulnerabilities**
- Identifies hardcoded credentials, SQL injection risks, unsanitized inputs.
```python
query = "SELECT * FROM users WHERE id = " + user_input  # SQL Injection
```

Tools: `bandit` (Python), `Brakeman` (Ruby), `semgrep`

---

#### 5. 🧼 **Dead Code and Unused Variables**
- Finds unused imports, unreachable code, and variables that are never referenced.
```python
import json  # unused
```

---

#### 6. 🧪 **Incorrect Type Usage**
- Type mismatches or violations in statically typed languages.
- Tools like `mypy` for Python can even help with dynamic type checking.

---

#### 7. 🧯 **Common Bugs and Anti-Patterns**
- Examples:
  - Using `==` instead of `===` in JavaScript
  - Assigning instead of comparing (`=` vs `==`)
  - Resource leaks (e.g., open file not closed)

---

#### 8. 🔁 **Duplicate Code**
- Highlights copy-pasted blocks which violate DRY (Don’t Repeat Yourself) principle.

Tool: `SonarQube`, `PMD`, `jscpd`

---

### 🧠 Real-World Example

In one of our Python projects:
- `bandit` detected a hardcoded AWS access key in a config file.
- `flake8` flagged missing docstrings and complex nested loops.
- `SonarQube` highlighted duplicated logic in two different modules.

These issues were caught **before** they were deployed to staging, saving time and preventing technical debt.

---

> Summary:  
> Static code analysis helps identify bugs, security risks, code smells, and style issues early in the development cycle. It boosts code quality, maintainability, and security without running the application.

---

- syntax errors
- unused variables
- style violations(indentation, naming conventions)
- Type mismatches
- code complexity(too many nested loops, long methods)
- some security issues.

