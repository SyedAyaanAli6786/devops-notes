# AWS and Terraform

## Terraform Basics
- Infrastructure as Code (IaC) tool
- Declarative configuration
- Manages cloud resources

## Core Concepts

### State File
- `terraform.tfstate`: Tracks managed infrastructure
- Use remote state (S3 + DynamoDB) for teams
- Never commit state file to Git

### Main Files
- `main.tf`: Primary configuration
- `variables.tf`: Input variables
- `outputs.tf`: Output values
- `terraform.tfvars`: Variable values

## AWS Networking

### VPC (Virtual Private Cloud)
- Isolated network in AWS
- Define CIDR block (e.g., 10.0.0.0/16)

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}
```

### Subnets
- **Public**: Internet gateway attached
- **Private**: No direct internet access

```hcl
resource "aws_subnet" "public" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
}
```

### Internet Gateway
- Allows VPC to communicate with internet

### Route Table
- Defines traffic routing rules

## IP Types
- **Private IP**: Internal VPC communication
- **Public IP**: Changes on restart
- **Elastic IP**: Static public IP (persists)

## EC2 Instance
```hcl
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.public.id
  
  tags = {
    Name = "WebServer"
  }
}
```

## Terraform Commands
```bash
# Initialize
terraform init

# Plan changes
terraform plan

# Apply changes
terraform apply

# Destroy resources
terraform destroy

# Format code
terraform fmt

# Validate configuration
terraform validate
```

## Dependencies
- Terraform automatically handles dependencies
- Reference resources: `aws_vpc.main.id`
- Explicit dependency: `depends_on = [aws_vpc.main]`

## Best Practices
- Use remote state
- Modularize configuration
- Use variables for reusability
- Tag all resources
- Version control Terraform files