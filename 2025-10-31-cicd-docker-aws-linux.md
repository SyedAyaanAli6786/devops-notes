# Class Notes – CI/CD, Docker, AWS & Linux



### Docker & Containers

- Docker packages app + dependencies.
- Docker image = static artifact.
- Images are built in layers.
- Layers are cached to save time.
- Images are stored in repositories.



### Deployment & Networking

- Public IP changes on restart.
- Elastic IP remains fixed.
- Elastic IP is better for production.
- Helps maintain stable access.



### AWS Fundamentals

- Region = Geographical area.
- AZ = Data center inside region.
- VPC = Private virtual network.
- Subnet = Smaller network inside VPC.
- Each subnet belongs to one AZ.
- Use multiple AZs for safety.



### EC2 Setup Fields

- Name & Tags → Identification
- AMI → Operating System
- Instance Type → CPU/RAM
- Key Pair → Login access
- Network → VPC, Subnet, Security Group
- Storage → EBS volumes
- IAM Role → Permissions
- User Data → Startup scripts
- Monitoring → CloudWatch



### Linux Fundamentals

- Linux is open-source OS.
- Kernel = Brain of system.
- Shell = Interface to kernel.
- Bash is most common shell.



### Basic Linux Commands

- pwd → Show path
- cd → Change directory
- mkdir → Create folder
- touch → Create file
- cat → View file
- echo → Print/write text
- history → Command history



### File Management

- ls → List files
- cp → Copy
- mv → Move/Rename
- rm → Delete
- find → Search
- du → Disk usage
- df → Free space



### Process Management

- ps → Show processes
- top / htop → Live monitoring
- kill → Stop process
- jobs → Background jobs
- fg / bg → Foreground/Background



### Permissions

- chmod → Change permission
- chown → Change owner
- chgrp → Change group



## Key Points

- CI/CD = Automation backbone
- Artifacts = Build output
- Docker = Portable deployment
- Elastic IP = Stable access
- VPC = Network isolation
- Subnets = Traffic control
- Linux = DevOps foundation
- Always be careful with `rm`
- Monitor processes regularly
- Permissions affect security