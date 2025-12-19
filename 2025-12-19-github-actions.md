# GitHub Actions

## What is GitHub Actions?
- CI/CD platform integrated with GitHub
- Automates build, test, and deployment
- Triggered by repository events

## Core Concepts

| Term | Description |
|------|-------------|
| Workflow | Automation defined in YAML |
| Job | Set of steps that run on same runner |
| Step | Individual task (action or command) |
| Runner | VM that executes jobs (Linux/Windows/Mac) |
| Action | Reusable code (e.g., checkout, setup-java) |

## Basic Workflow Structure
```yaml
name: Java Build Workflow

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
      
      - name: Build with Maven
        run: mvn clean package
```

## Common Triggers
```yaml
on:
  push:              # On push to repo
  pull_request:      # On PR creation
  schedule:          # Cron schedule
    - cron: '0 0 * * *'
  workflow_dispatch: # Manual trigger
```

## Useful Actions
- `actions/checkout@v4`: Clone repository
- `actions/setup-java@v4`: Install Java
- `actions/setup-python@v4`: Install Python
- `actions/setup-node@v4`: Install Node.js
- `docker/login-action@v3`: Login to Docker Hub

## Secrets
Store sensitive data in GitHub Secrets:
- Settings → Secrets → Actions → New secret
- Access in workflow: `${{ secrets.SECRET_NAME }}`

## Viewing Results
- GitHub Repo → Actions tab
- Click workflow run to see logs
- Green check = success, Red X = failure