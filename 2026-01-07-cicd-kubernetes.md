# CI/CD with Kubernetes

## CI vs CD
- **CI (Continuous Integration)**: Build, test, package
- **CD (Continuous Deployment)**: Deploy to production

## Deployment Approaches

### 1. GitHub Actions + kubectl
```yaml
- name: Deploy to Kubernetes
  run: |
    kubectl apply -f deployment.yaml
    kubectl rollout status deployment/myapp
```

### 2. GitHub Actions + Custom Runner
- Self-hosted runner on K8s cluster
- Direct access to cluster
- No need for kubeconfig secrets

### 3. GitOps (ArgoCD/Flux)
- Git as source of truth
- Automatic sync from Git to cluster
- Declarative deployments

## Example CD Workflow
```yaml
name: Deploy to Kubernetes

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up kubectl
        uses: azure/setup-kubectl@v3
      
      - name: Configure kubeconfig
        run: |
          echo "${{ secrets.KUBECONFIG }}" > kubeconfig
          export KUBECONFIG=kubeconfig
      
      - name: Deploy
        run: |
          kubectl apply -f k8s/
          kubectl rollout status deployment/myapp
```

## Deployment Strategies
- **Rolling Update**: Gradual replacement (default)
- **Blue-Green**: Switch between two environments
- **Canary**: Test with small percentage of traffic

## Best Practices
- Use separate environments (dev, staging, prod)
- Implement health checks
- Use resource limits
- Monitor deployments
- Have rollback plan