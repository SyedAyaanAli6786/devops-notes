# Kubernetes Pods

## What is a Pod?
- Smallest deployable unit in Kubernetes
- Contains one or more containers sharing network namespace, IPC, and volumes
- Ephemeral - can be created/destroyed easily

## Key Components

### Control Plane
- **API Server**: Entry point for all operations, validates requests, stores in etcd
- **etcd**: Key-value store for cluster state
- **Scheduler**: Assigns Pods to Nodes
- **Controller Manager**: Runs controllers (ReplicaSet, Deployment, etc.)

### Data Plane
- **kubelet**: Node agent that manages containers
- **Container Runtime**: Pulls images and creates containers (containerd/CRI-O)
- **kube-proxy**: Manages networking rules
- **CNI**: Sets up Pod network interfaces

## Pod Creation Flow

1. `kubectl apply -f pod.yaml` → API Server
2. API Server authenticates/authorizes
3. Admission controllers validate/mutate
4. Pod stored in etcd
5. Scheduler assigns Pod to Node
6. kubelet creates containers
7. Pod becomes Running/Ready

## Pod Storage in etcd
```
/registry/pods/<namespace>/<podName>
```

**Key fields:**
- `spec`: containers, volumes, nodeName
- `status`: phase, conditions, containerStatuses
- `metadata`: uid, resourceVersion

## Pod Lifecycle States
- **Pending**: Waiting for scheduling
- **Running**: Containers are running
- **Succeeded**: All containers completed successfully
- **Failed**: At least one container failed
- **Unknown**: Pod state cannot be determined

## Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| ImagePullBackOff | Invalid image/credentials | Check image name and pullSecrets |
| CrashLoopBackOff | Container keeps crashing | Check logs and liveness probe |
| Pending | No resources/scheduling issues | Check node capacity and taints |
| ContainerCreating | CNI/volume issues | Check CNI and CSI logs |

## Useful Commands

```bash
# View pods
kubectl get pods -A

# Describe pod
kubectl describe pod <pod> -n <ns>

# View logs
kubectl logs <pod> -c <container>

# Watch pod status
kubectl get pods -w

# Check events
kubectl get events -n <ns> --sort-by='.lastTimestamp'
```

## Best Practices
- Use Deployments/ReplicaSets instead of bare Pods
- Define resource requests and limits
- Use readiness probes for traffic management
- Use liveness probes for auto-recovery
- Set proper affinity/anti-affinity rules