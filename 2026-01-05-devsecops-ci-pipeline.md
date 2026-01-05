# DevSecOps CI Pipeline

## What is DevSecOps?
- Security integrated into DevOps pipeline
- "Shift-left" security (test early)
- Automated security scanning

## CI Pipeline Stages

### 1. Code Checkout
```yaml
- uses: actions/checkout@v4
```

### 2. Build
```yaml
- name: Build with Maven
  run: mvn clean package
```

### 3. Linting (Code Quality)
```yaml
- name: Lint code
  run: mvn checkstyle:check
```

### 4. Unit Tests
```yaml
- name: Run tests
  run: mvn test
```

### 5. SAST (Static Application Security Testing)
- Scans source code for vulnerabilities
- Tools: CodeQL, SonarQube
- Detects: SQL injection, XSS, insecure deserialization

```yaml
- name: CodeQL Analysis
  uses: github/codeql-action/analyze@v2
```

### 6. SCA (Software Composition Analysis)
- Scans dependencies for known vulnerabilities
- Tools: OWASP Dependency Check, Snyk
- Checks: CVEs in libraries

```yaml
- name: Dependency Check
  uses: dependency-check/Dependency-Check_Action@main
```

### 7. Build Docker Image
```yaml
- name: Build Docker image
  run: docker build -t myapp:latest .
```

### 8. Container Scanning
- Scans Docker image for vulnerabilities
- Tool: Trivy

```yaml
- name: Trivy scan
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: myapp:latest
    exit-code: 1  # Fail on critical/high
```

### 9. Container Testing
```yaml
- name: Test container
  run: |
    docker run -d -p 8080:8080 myapp:latest
    sleep 5
    curl http://localhost:8080
```

### 10. Push to Registry
```yaml
- name: Login to DockerHub
  uses: docker/login-action@v3
  with:
    username: ${{ secrets.DOCKERHUB_USERNAME }}
    password: ${{ secrets.DOCKERHUB_TOKEN }}

- name: Push image
  run: docker push myapp:latest
```

## Security Layers
1. **Code**: SAST (CodeQL)
2. **Dependencies**: SCA (Dependency Check)
3. **Container**: Image scanning (Trivy)
4. **Runtime**: Container testing

## Best Practices
- Fail pipeline on critical vulnerabilities
- Use secrets for credentials
- Scan at multiple stages
- Upload results to GitHub Security tab
- Automate everything