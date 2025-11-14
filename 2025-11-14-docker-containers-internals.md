# Class Notes – Docker Containers & Internals



## ✅ What is a Container?

- A container = **Linux process + isolation**.
- It is **not a VM**.
- It shares host OS kernel.
- Runs like a normal process with restrictions.
- Container = Process with fake environment.



## ✅ History (chroot)

- Earlier, `chroot` created file system jails.
- Containers evolved from this idea.
- Now use advanced kernel features.



## ✅ Containers vs Virtual Machines

| Feature | Container | VM |
| --- | --- | --- |
| OS | Shared | Separate |
| Speed | Very Fast | Slow |
| Size | Small | Large |
| Kernel | Host Kernel | Own Kernel |
| Boot | ms | Minutes |
- Containers are lighter than VMs.



## ✅ How Containers Work

Containers use Linux features:

### 1️⃣ Namespaces (Isolation)

Namespaces hide parts of system.

| Namespace | Purpose |
| --- | --- |
| PID | Process IDs |
| NET | Network |
| MOUNT | Filesystem |
| UTS | Hostname |
| IPC | Shared memory |
| USER | User IDs |
- Makes container feel like separate machine.



### 2️⃣ Cgroups (Resource Control)

Cgroups limit resources:

- CPU
- Memory
- Disk
- PIDs

Example:

- 256MB RAM
- 0.5 CPU
- Prevents one container from using all resources.



## ✅ Container = Just a Process

When you run:

```
docker run nginx

```

Docker starts:

- A Linux process
- Inside namespaces
- With cgroups
- No virtual machine is created.



## ✅ Docker Internal Architecture

Flow:

```
docker → containerd → shim → runc → process

```

### containerd

- Manages containers
- Handles images & network

### shim

- One per container
- Keeps container alive

### runc

- Creates namespaces & cgroups
- Starts process



## ✅ Process Tree Example

```
containerd
 └─ shim
    └─ nginx

```

- Inside container → nginx = PID 1
- On host → normal PID
- PID 1 controls container life.



## ✅ Why Containers are Ephemeral

Container filesystem:

- Read-only image layers
- Temporary writable layer

When container stops:

- Writable layer deleted
- Data lost
- Containers are temporary.

Data must be stored in:

- Volumes
- Databases
- External storage



## ✅ Why Killing Main Process Stops Container

- Container = PID 1
- If PID 1 dies → container dies

Example:

```
dockerkill <id>

```

→ Sends signal to PID 1

→ Container stops



## ✅ Why Isolation is “Pseudo-Isolation”

- All containers share kernel
- Processes visible in `ps -ef`
- Root in container ≠ real root
- Kernel bugs can break isolation
- Not as strong as VM isolation.



## ✅ Why Containers Start So Fast

Because:

- No OS boot
- No BIOS
- No kernel load
- Only starts process

Start time: ~50ms

VM: 30–60 sec



## ✅ Why Containers are Lightweight

Containers share:

- Kernel
- OS
- CPU scheduler
- Memory manager

Only isolation is separate.
- Less storage & RAM usage.



## ✅ Why Containers Fail When App Exits

Container runs **one main process**.

If app stops:

- Container stops

Examples:

- nginx crash → container stops
- python exits → container stops
- Beginners often get confused.



## ✅ Port Mapping & Networking

Containers have own network.

Port mapping:

```
docker run -p8080:80 nginx

```

Host:8080 → Container:80
- Used to access apps.



## ✅ Namespaces Create “Illusion”

Containers feel separate because:

- Own PID list
- Own IP
- Own hostname
- Own root directory
- Own users

But all are fake views.



## ✅ How Docker Creates a Container (Steps)

1. Pull image
2. Load layers
3. Create cgroups
4. Create namespaces
5. Setup filesystem
6. Start process
- docker run = many kernel steps.



## ✅ Why Normal Programs Don’t Feel Like Containers

Normal programs share:

- Network
- Filesystem
- Hostname
- PIDs
- Resources

Containers hide all this.



## ✅ VM vs Container (Deep View)

### VM

```
Hardware → Hypervisor → Guest OS → App

```

### Container

```
Hardware → Linux Kernel → App

```
- Containers don’t run their own OS.



## Key Points

- Container = Linux process
- Uses namespaces + cgroups
- No separate OS
- PID 1 controls container
- Data is temporary
- Isolation is limited
- Very fast startup
- Lightweight
- Shares kernel
- Not a VM



## 📚 Quick Summary
- Containers are isolated processes
- Namespaces = isolation
- Cgroups = limits
- PID 1 = container life
- Writable layer is temporary
- Data needs volumes
- Faster than VMs
- Less secure than VMs