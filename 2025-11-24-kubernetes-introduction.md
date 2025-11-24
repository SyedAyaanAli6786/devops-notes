# Kubernetes Introduction

## What is Kubernetes?
- Open-source container orchestration platform
- Automates deployment, scaling, and management of containerized applications
- "Operating system for your data center"

## Key Features
- **Self-healing**: Automatically restarts failed containers
- **Auto-scaling**: Scales based on demand
- **Load balancing**: Distributes traffic across pods
- **Rolling updates**: Zero-downtime deployments
- **Storage orchestration**: Manages persistent storage

## Architecture

### Control Plane (Master)
- **API Server**: Central management point, handles all requests
- **etcd**: Key-value store for cluster state
- **Scheduler**: Assigns pods to nodes
- **Controller Manager**: Runs control loops (Deployment, ReplicaSet, etc.)
- **Cloud Controller**: Integrates with cloud providers

### Worker Nodes
- **kubelet**: Node agent, manages containers
- **kube-proxy**: Network proxy, handles service routing
- **Container Runtime**: Runs containers (containerd, CRI-O)

## Core Concepts
- **Pod**: Smallest unit, one or more containers
- **Service**: Stable endpoint for pods
- **Deployment**: Manages ReplicaSets and rolling updates
- **ReplicaSet**: Ensures desired number of pod replicas
- **Namespace**: Virtual cluster for resource isolation

## Why Kubernetes?
- Handles container failures automatically
- Scales applications easily
- Manages complex deployments
- Works across cloud providers
- Industry standard for container orchestration