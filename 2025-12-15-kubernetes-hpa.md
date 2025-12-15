# Horizontal Pod Autoscaler (HPA)

## What is HPA?
- Automatically scales pod replicas based on metrics
- Monitors CPU/memory or custom metrics
- Increases/decreases pods as needed

## Requirements
- **Metrics Server** must be installed
- Pods must have resource requests defined

## HPA Example
```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: hpa-demo
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: hpa-demo-deployment
  minReplicas: 1
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

## How It Works
1. Metrics Server collects pod metrics
2. HPA controller checks average CPU/memory
3. If above target → scales up
4. If below target → scales down (after cooldown)

## Setup Steps

### 1. Install Metrics Server
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

### 2. Create Deployment with Resource Requests
```yaml
resources:
  requests:
    cpu: 200m
  limits:
    cpu: 500m
```

### 3. Create HPA
```bash
kubectl apply -f hpa.yaml
```

### 4. Generate Load
```bash
kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- \
  /bin/sh -c "while sleep 0.01; do wget -q -O- http://service-name; done"
```

## Monitoring
```bash
# Watch HPA
kubectl get hpa -w

# Check pod CPU
kubectl top pods

# View scaling events
kubectl describe hpa hpa-demo
```

## Key Points
- HPA reacts to average pod metrics
- Scaling is gradual (not instant)
- Cooldown period prevents flapping
- CPU calculation: `Usage / Request`
- Works with Deployments, ReplicaSets, StatefulSets