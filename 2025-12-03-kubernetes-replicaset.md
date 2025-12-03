# Kubernetes ReplicaSet

## What is a ReplicaSet?
- Controller that ensures a specified number of pod replicas are running
- Automatically creates/deletes pods to match desired state
- Usually managed by Deployments (not created directly)

## How It Works
1. Watches for pods matching its label selector
2. Compares actual count vs desired count
3. Creates pods if too few
4. Deletes pods if too many

## YAML Example
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx:latest
```

## Creation Flow
1. `kubectl apply` → API Server
2. API Server stores in etcd
3. ReplicaSet controller sees new RS
4. Controller creates 3 pods
5. Scheduler assigns pods to nodes
6. kubelet starts containers

## Failure Handling

| Scenario | ReplicaSet Action |
|----------|-------------------|
| Pod crashes | Creates replacement |
| Node dies | Recreates pods on healthy nodes |
| Manual pod deletion | Creates new pod |
| Scale up | Creates additional pods |
| Scale down | Deletes excess pods |

## Common Commands
```bash
# Create ReplicaSet
kubectl apply -f replicaset.yaml

# List ReplicaSets
kubectl get rs

# Scale ReplicaSet
kubectl scale rs nginx-rs --replicas=5

# Delete ReplicaSet
kubectl delete rs nginx-rs
```

## Key Points
- ReplicaSets ensure high availability
- Don't create ReplicaSets directly - use Deployments
- Uses label selectors to identify pods
- Stored in etcd at `/registry/replicasets/<namespace>/<name>`