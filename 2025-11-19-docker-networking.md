# Docker Networking

## Networking Basics

### IP Address
- Unique address for devices on a network

### NIC (Network Interface Card)
- Hardware that connects computer to network
- Converts data to electrical signals
- Adds MAC addresses to frames

### Network Switch
- Layer-2 device connecting devices in LAN
- Forwards frames based on MAC addresses
- Maintains MAC address table

### ARP (Address Resolution Protocol)
- Maps IP address → MAC address
- Process: Broadcast "Who has IP X?" → Device replies with MAC

### Gateway
- Router for traffic outside the subnet
- Default gateway handles external traffic

### NAT (Network Address Translation)
- **SNAT**: Changes source IP (private → public)
- **DNAT**: Changes destination IP (port forwarding)

## Docker Networking

### Network Namespace
- Each container has isolated network stack
- Own network interface, routing table, firewall rules

### veth Pairs
- Virtual ethernet cables connecting container to bridge
- One end in container (eth0), other on host bridge

### Docker Bridge (docker0)
- Default virtual switch for containers
- IP range: 172.17.0.0/16
- Containers get internal IPs and can communicate

## Docker Network Types

| Type | Use Case | Isolation |
|------|----------|-----------|
| **bridge** | Default, local development | Isolated network |
| **host** | High performance, no NAT | No isolation |
| **none** | Complete isolation | No network |
| **overlay** | Multi-host (Swarm/K8s) | Cross-host |
| **macvlan** | Container as real LAN device | Own MAC address |

## Common Commands

```bash
# Create custom network
docker network create mynet

# Run container in network
docker run -d --name app1 --network mynet nginx

# List networks
docker network ls

# Inspect network
docker network inspect bridge

# Port mapping
docker run -p 8080:80 nginx  # Host:8080 → Container:80
```

## Custom Bridge vs Default Bridge

**Default Bridge:**
- No DNS-based service discovery
- Must use IP addresses or links

**Custom Bridge:**
- Built-in DNS (containers reach each other by name)
- Better isolation
- Recommended for production

## Container Internet Access
- Traffic: Container → docker0 → host eth0 → Internet
- Uses NAT (iptables MASQUERADE)
- Return traffic routed back to container

## Example: App + Database

```bash
# Create network
docker network create backend

# Run database
docker run -d --name db --network backend mysql

# Run app (connects to db:3306)
docker run -d --name app --network backend myapp
```