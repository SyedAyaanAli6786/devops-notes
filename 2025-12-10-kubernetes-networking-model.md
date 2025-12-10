# Kubernetes Networking Model

## Core Rules
1. All pods can communicate without NAT
2. All nodes can communicate with all pods without NAT
3. Pod's IP is same inside and outside

## Networking Layers

### 1. Container-to-Container (Same Pod)
- Containers share network namespace
- Communicate via `localhost`
- Share same IP and ports

### 2. Pod-to-Pod (Same Node)
- Each pod has own IP
- Connected via veth pairs to bridge
- Bridge forwards traffic between pods

### 3. Pod-to-Pod (Different Nodes)
- Each node has Pod CIDR range
- Traffic routed through network
- CNI plugin handles routing

### 4. Pod-to-Service
- Service provides stable virtual IP
- kube-proxy creates iptables/IPVS rules
- Traffic load-balanced to pod IPs

### 5. External-to-Service
- **Egress** (Pod → Internet): SNAT (Pod IP → Node IP)
- **Ingress** (Internet → Pod):
  - LoadBalancer: Cloud LB → NodePort → Pods
  - Ingress Controller: Layer 7 routing

## CNI (Container Network Interface)
- Assigns IP to each pod
- Creates network interfaces
- Popular CNIs: Calico, Flannel, Weave, Cilium

## kube-proxy Modes
- **iptables**: Classic, uses NAT rules
- **IPVS**: Modern, faster, kernel-level load balancing

## DNS (CoreDNS)
- Automatic DNS for services
- Format: `<service>.<namespace>.svc.cluster.local`

## Summary

| Layer | Mechanism |
|-------|-----------|
| Container-to-Container | Shared namespace, localhost |
| Pod-to-Pod (same node) | veth → bridge → veth |
| Pod-to-Pod (cross node) | Routed via Pod CIDR |
| Pod-to-Service | iptables/IPVS DNAT |
| Egress | SNAT (Pod IP → Node IP) |
| Ingress | LoadBalancer or Ingress Controller |