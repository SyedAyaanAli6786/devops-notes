# Kubernetes Services

## Why Services?
- Pods have dynamic IPs (change on restart)
- Services provide stable endpoint
- Load balance across pod replicas

## Service Types

### ClusterIP (Default)
- Internal cluster communication only
- Stable virtual IP
- DNS name: `<service>.<namespace>.svc.cluster.local`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-svc
spec:
  type: ClusterIP
  selector:
    app: backend
  ports:
  - port: 80
    targetPort: 8080
```

### NodePort
- Exposes service on each node's IP
- Port range: 30000-32767
- Access: `<NodeIP>:<NodePort>`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-nodeport
spec:
  type: NodePort
  selector:
    app: backend
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 32080
```

### LoadBalancer
- Cloud provider load balancer
- Public IP assigned
- Production use

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-lb
spec:
  type: LoadBalancer
  selector:
    app: backend
  ports:
  - port: 80
    targetPort: 8080
```

## How Services Work
- kube-proxy creates iptables/IPVS rules
- Traffic to Service IP → routed to pod IPs
- Load balancing across healthy pods

## DNS Resolution
Every service gets DNS:
- Short: `backend-svc`
- Full: `backend-svc.default.svc.cluster.local`

## Common Commands
```bash
# Create service
kubectl apply -f service.yaml

# List services
kubectl get svc

# Describe service
kubectl describe svc backend-svc

# Get endpoints
kubectl get endpoints backend-svc
```

## When to Use

| Need | Service Type |
|------|--------------|
| Internal microservices | ClusterIP |
| Local testing/debugging | NodePort |
| Production external access | LoadBalancer |
| Databases | Headless (clusterIP: None) |