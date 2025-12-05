# Kubernetes Workloads

## Pod
- Smallest deployable unit
- One or more containers sharing network/storage
- Ephemeral - not self-healing alone

## ReplicaSet
- Ensures X number of pod replicas
- Self-healing
- No rolling updates

## Deployment
- **Recommended for stateless apps**
- Manages ReplicaSets
- Rolling updates and rollbacks
- Declarative updates

### Deployment Example
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: app
        image: nginx:1.27
```

### Update Strategy
- **RollingUpdate** (default): Gradual replacement
  - `maxUnavailable`: Max pods down during update
  - `maxSurge`: Max extra pods during update
- **Recreate**: Delete all, then create new

### Deployment Commands
```bash
# Create
kubectl apply -f deployment.yaml

# Update image
kubectl set image deployment/webapp app=nginx:1.28

# Rollout status
kubectl rollout status deployment/webapp

# Rollback
kubectl rollout undo deployment/webapp

# Scale
kubectl scale deployment webapp --replicas=10
```

## StatefulSet
- For stateful applications (databases)
- Stable network identities
- Ordered deployment/scaling
- Persistent storage per pod

## DaemonSet
- Runs one pod per node
- Use cases: logging, monitoring, network plugins

## Job
- Runs pods to completion
- For batch processing

## CronJob
- Scheduled jobs
- Like cron in Linux

## Comparison

| Workload | Use Case | Self-Healing | Updates |
|----------|----------|--------------|---------|
| Pod | Testing only | No | Manual |
| ReplicaSet | Rarely used directly | Yes | Manual |
| Deployment | Stateless apps | Yes | Rolling |
| StatefulSet | Databases | Yes | Ordered |
| DaemonSet | Node services | Yes | Rolling |
| Job | Batch tasks | No | N/A |
| CronJob | Scheduled tasks | No | N/A |