# Docker Image & Dockerfile – Short Notes



## 📝 Part 1: Docker Image

### What is a Docker Image?

- Docker image = Read-only package of an application.
- Contains:
    - Code
    - Runtime
    - Libraries
    - Config files
- Built using a **Dockerfile**.
- Stored in layers.
- Image = Template
- Container = Running image



### Image → Container

- When we run:

```
docker run image_name

```

- Docker creates a container.
- Container = Image + Writable layer.
- Changes happen only in container.



## 📝 Part 2: Dockerfile Fundamentals

### What is Dockerfile?

- Dockerfile = Recipe to build image.
- Contains step-by-step instructions.

Example:

```
FROM python:3.12-slim
WORKDIR/app
COPY . .
RUN pip install-r requirements.txt
CMD ["python","app.py"]

```



## 📌 Important Dockerfile Instructions



### 1️⃣ FROM — Base Image

- Starting point of image.
- Must be first instruction.

Syntax:

```
FROMimage:tag

```

Examples:

```
FROM ubuntu:22.04
FROM python:3.12-slim

```
- Use slim/alpine images
- Always pin versions



### 2️⃣ RUN — Build Commands

- Executes commands during build.
- Creates a new layer.

Example:

```
RUN apt-getupdate&& apt-get install curl

```
- Combine commands
- Clean cache
- Reduce layers



### 3️⃣ CMD — Default Command

- Runs when container starts.
- Can be overridden.

Syntax:

```
CMD["python","app.py"]

```
- Use JSON form
- Only one CMD allowed



### 4️⃣ ENTRYPOINT — Main Program

- Fixed main command.
- Always runs.

Example:

```
ENTRYPOINT["python","app.py"]

```
- CMD gives arguments to ENTRYPOINT.



### 5️⃣ ENTRYPOINT + CMD Together

Concept:

- ENTRYPOINT = Program
- CMD = Arguments

Example:

```
ENTRYPOINT["echo"]
CMD["Hello"]

```

Result:

```
Hello

```



### 6️⃣ COPY vs ADD

### COPY (Preferred)

- Simple file copy.

```
COPY . /app

```

### ADD

- Supports URL & tar extract.

```
ADD file.tar.gz /app

```
- Use COPY normally
- Use ADD only if needed



### 7️⃣ ENV — Environment Variables

- Sets environment values.

Example:

```
ENV PORT=8080

```
- For fixed config
- Use `-e` for dynamic values



## 📝 Part 3: Real Project Example (Flask App)

### Folder Structure

```
flask-app/
 app.py
 requirements.txt
 Dockerfile

```



### Dockerfile Example

```
FROM python:3.12-slim
WORKDIR/app
COPY requirements.txt .
RUN pip install-r requirements.txt
COPY . .
ENV PORT=8080
EXPOSE8080
ENTRYPOINT ["python"]
CMD ["app.py"]

```



### Build & Run

```
docker build -t flask-app .
docker run -p8080:8080 flask-app

```

Access:

```
http://localhost:8080

```



## 📝 Part 4: Docker Image Best Practices

| Area | Best Practice | Why |
| --- | --- | --- |
| Base Image | Use slim/alpine | Smaller size |
| Layers | Combine RUN | Less layers |
| Caching | Copy deps first | Faster build |
| Cleanup | Remove cache | No bloat |
| Security | Use non-root | Safer |
| Secrets | Use ENV | No hardcoding |
| Versions | Pin versions | Stable builds |
| Multi-stage | Use stages | Smaller image |
| Healthcheck | Add check | Auto-restart |



### Secure Image Example

```
FROM python:3.12-slim
WORKDIR/app
COPY requirements.txt .
RUN pip install-r requirements.txt&& useradd appuser
COPY . .
USER appuser
ENTRYPOINT ["python"]
CMD ["app.py"]

```
- Runs as non-root
- More secure



## 📝 Part 5: Inspecting & Analyzing Images

### View Layers

```
docker history image

```

### Inspect Details

```
docker inspect image

```

### Security Scan

```
docker scan image
trivy image image

```
- Used to find vulnerabilities.



## Key Points

- Image = Read-only template
- Container = Running image
- Dockerfile builds images
- FROM is mandatory
- RUN creates layers
- CMD = Default command
- ENTRYPOINT = Fixed command
- COPY > ADD (mostly)
- Multi-stage = Small image
- Security is important



## 📚 Quick Summary
- Docker image = Packaged app
- Dockerfile = Build guide
- Layers = Performance factor
- CMD + ENTRYPOINT control startup
- Best practices = Faster + Safer images
- Inspect & scan before production