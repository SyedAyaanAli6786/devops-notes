# Class Notes – Docker & Containerization (Today)



### Docker Philosophy

- “If it fits, it ships”
- If app runs in container → it will run everywhere.
- No environment issues.



### Containers

- Lightweight virtual environment.
- Contains:
    - App
    - Libraries
    - Tools
    - Settings
- Uses host kernel (not full OS like VM).
- Faster than virtual machines.



### Images

- Image = Template for container.
- Read-only file.
- Used to create containers.

Example:

```
docker run ubuntu

```



### Docker Client & Daemon

### Docker Client

- CLI tool (`docker` command)
- Sends commands

### Docker Daemon

- Runs in background
- Manages containers & images
- Client talks to Daemon.



### Volumes & Networking

### Volumes

- Store data permanently.
- Data is safe even if container stops.

### Networking

- Allows container-to-container communication.
- Connects containers to internet.



### Running Containers

- Main command:

```
docker run

```

- Interactive mode:

```
docker run -it ubuntu

```

Flags:

- `i` → Interactive
- `t` → Terminal



### Docker Hub

- Online image repository.
- Stores public/private images.
- Default image source.

Example:

```
docker pull nginx

```



### Practice in Class

- Ran containers in Cloud Shell.
- Pulled images from Docker Hub.
- Tested basic commands.
- Learned container deployment.
- Focus on hands-on learning.



### Docker in Modern DevOps

- Docker is foundation tool.
- Used with Kubernetes.
- Production use reduced.
- Still very important for learning.



## Key Points

- Docker = Container platform
- Image = Blueprint
- Container = Running instance
- Daemon runs containers
- Volumes store data
- Docker Hub stores images
- Containers are lightweight
- Used before Kubernetes