# Class Notes – Docker & Containerization (Today)



### Containers

- Lightweight isolated environment.
- Includes code, libraries, runtime, config.
- Uses host OS kernel (faster than VM).
- Faster and efficient than Virtual Machines.



### Images

- Image = Blueprint / Template.
- Used to create containers.
- Read-only.

Example:

```
docker run ubuntu

```



### Docker Architecture

- Docker follows Client–Server model.

Flow:

CLI → Daemon → Containers → Host OS

- Client sends commands.
- Daemon executes them.
- Runtime manages containers.



### Basic Docker Commands

| Task | Command |
| --- | --- |
| Run container | docker run |
| List running | docker ps |
| List all | docker ps -a |
| List images | docker images |
| Pull image | docker pull |
| Stop | docker stop |
| Delete container | docker rm |
| Delete image | docker rmi |



### Running Containers

Interactive:

```
docker run -it ubuntu bash

```

Detached:

```
docker run -d nginx

```

Flags:

- `i` → Interactive
- `t` → Terminal
- `d` → Background



### Entering Running Container

- Used when container is already running.

```
dockerexec -it <id> bash

```



### Docker Hub

- Online repository for images.
- Default image source.
- Like GitHub for Docker.

Example:

```
docker pull nginx
docker push username/image

```



### Networking & Communication

- Each container has an IP.
- Find using:

```
docker inspect

```

- Can connect using curl or APIs.
- Used for microservices.



### Dockerfile & Layers

- Dockerfile = Instructions to build image.
- Each command creates a layer.

Example:

```
FROM ubuntu
RUN apt install nginx
COPY ./app

```
- More layers = bigger image.



### Layered File System

- Uses Union File System.
- Read-only image layers.
- One writable container layer.

Structure:

- Base OS
- App layers
- Config layers
- Writable layer



### Copy-on-Write

- File modified → copied to writable layer.
- Original layer remains unchanged.
- Saves storage.



### Image Storage (Linux)

- Stored in:

```
/var/lib/docker/overlay2/

```

- Contains layer data and metadata.



### Inspecting Layers

- View history:

```
docker history image

```

- Inspect image:

```
docker image inspect

```



### Caching & Reuse

- Layers have SHA256 hash.
- Same layers reused.
- Saves space and time.
- Faster builds.



### VM vs Docker

| Feature | VM | Docker |
| --- | --- | --- |
| OS | Full OS | Shared OS |
| Speed | Slow | Fast |
| Size | Large | Small |
| Boot | Minutes | Seconds |
- Docker is lightweight.



### Multi-Architecture Issue

- ARM ≠ AMD
- Images may not run everywhere.
- Must match system architecture.



## Key Points

- Image = Template
- Container = Running app
- Daemon manages everything
- Dockerfile builds images
- Layers improve efficiency
- Volumes save data
- Docker Hub stores images
- Containers ≠ Virtual Machines



## Practice Work

- Run ubuntu container
- Build image using Dockerfile
- Push image to Docker Hub
- Inspect layers
- Test networking
- Practice exec & detach mode