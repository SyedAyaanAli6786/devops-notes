# Class Notes – Docker Volumes (Today)



## 📝 Why Do We Need Volumes?

### Problem with Containers

- Containers are **ephemeral** (temporary).
- They have:
    - Read-only image
    - Writable layer
- When container is removed → writable layer is deleted.
- Data is lost.

Examples of lost data:

```
/var/lib/mysql
/app/logs
/usr/share/nginx/html

```
- To save data → use **Volumes or Bind Mounts**.



## 📝 Types of Docker Volumes

There are **4 types**:

1. Anonymous Volume
2. Named Volume
3. Bind Mount
4. tmpfs Mount



## 1️⃣ Anonymous Volume

### What is it?

- Created automatically by Docker.
- Has random name.
- Not managed manually.

Example:

```
docker run -d -v /data nginx

```

### Features

- Random name
- Temporary
- Deleted with `-rm`

### Use Case

- Testing
- Short-lived containers



## 2️⃣ Named Volume (Most Important)

### What is it?

- Created by user.
- Has fixed name.
- Managed by Docker.
- Data stays even after container is deleted.

Create volume:

```
docker volumecreate mydata

```

Use in container:

```
docker run -d -v mydata:/var/lib/mysql mysql

```

### Features

- Persistent storage
- Safe
- Production-ready

### Use Case

- Databases
- Uploads
- App storage
- Best for production.



## 3️⃣ Bind Mount

### What is it?

- Maps host folder → container folder.
- Uses real system path.

Format:

```
-v /host/path:/container/path

```

Example:

```
docker run -d \
-v $(pwd)/site:/usr/share/nginx/html \
nginx

```

### Features

- Live sync
- Host controls data
- Needs absolute path

### Use Case

- Development
- Logs
- Config files
- Certificates
- Best for local development.



## 4️⃣ tmpfs Mount

### What is it?

- Stored in RAM.
- Not saved on disk.
- Deleted when container stops.

Example:

```
docker run -d --tmpfs /cache nginx

```

### Features

- Very fast
- No persistence
- Secure

### Use Case

- Cache
- Secrets
- Temporary files
- Best for sensitive data.



## 📝 Comparison Table (Important)

| Type | Persistent | Stored Where | Best Use |
| --- | --- | --- | --- |
| Anonymous | Yes (short time) | Docker folder | Testing |
| Named | Yes | Docker folder | Database |
| Bind | On Host | Host system | Development |
| tmpfs | No | RAM | Cache/Security |



## 📝 Where Are Volumes Stored?

### Default Location (Linux)

```
/var/lib/docker/volumes/

```

Example:

```
/var/lib/docker/volumes/mydata/_data/

```
- Docker manages this folder.

⚠ Bind mounts are NOT stored here.



## 📝 Real-World Use Cases

### ✔ Named Volume

- MySQL
- MongoDB
- PostgreSQL
- Redis
- File uploads

### ✔ Bind Mount

- Live code editing
- Log files
- Config injection

### ✔ tmpfs

- Passwords
- Tokens
- Build cache

### ✔ Anonymous

- Test containers
- Disposable apps



## 📝 Examples



### 1️⃣ Anonymous Volume

```
docker run -d -v /logs myapp

```



### 2️⃣ Named Volume

```
docker volume create mydb
docker run -d -v mydb:/var/lib/mysql mysql

```



### 3️⃣ Bind Mount

```
mkdir website
docker run -d \
-v $(pwd)/website:/usr/share/nginx/html \
nginx

```



### 4️⃣ tmpfs

```
docker run -d --tmpfs /secret nginx

```



## Key Points

- Containers delete data on removal
- Volumes save data
- Named volume = Best choice
- Bind mount = Development use
- tmpfs = RAM only
- Anonymous = Temporary
- Docker stores volumes in `/var/lib/docker/volumes`
- Databases must use volumes