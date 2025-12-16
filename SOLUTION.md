# SimpleTimeService

A minimal microservice that returns the current timestamp and visitor's IP address in JSON format.

## Project Structure

```
.
├── app/                    # Application source code
│   ├── main.go            # Go application
│   ├── go.mod             # Go module file
│   └── Dockerfile         # Multi-stage Dockerfile
└── terraform/             # Infrastructure as Code
    ├── main.tf            # Main Terraform configuration
    ├── variables.tf       # Variable definitions
    ├── outputs.tf         # Output definitions
    └── terraform.tfvars   # Variable values
```

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Terraform](https://developer.hashicorp.com/terraform/downloads) >= 1.0
- [AWS CLI](https://aws.amazon.com/cli/) configured with appropriate credentials
- Docker Hub account (for publishing the image)

## Task 1: Application & Docker

### Build the Docker Image

```bash
cd app
docker build -t simpletimeservice .
```

### Run Locally

```bash
docker run -p 8080:8080 simpletimeservice
```

Test: `curl http://localhost:8080/`

### Publish to Docker Hub

```bash
docker tag simpletimeservice YOUR_DOCKERHUB_USERNAME/simpletimeservice:latest
docker push YOUR_DOCKERHUB_USERNAME/simpletimeservice:latest
```

## Task 2: Terraform Infrastructure

### AWS Authentication

Configure AWS credentials using one of these methods:

```bash
# Option 1: Environment variables
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_REGION="us-east-1"

# Option 2: AWS CLI profile
aws configure
```

### Deploy Infrastructure

1. Update `terraform/terraform.tfvars` with your Docker Hub image:
   ```hcl
   container_image = "YOUR_DOCKERHUB_USERNAME/simpletimeservice:latest"
   ```

2. Initialize and deploy:
   ```bash
   cd terraform
   terraform init
   terraform plan
   terraform apply
   ```

3. Access the application using the ALB DNS name from the output:
   ```bash
   curl http://<alb_dns_name>/
   ```

### Destroy Infrastructure

```bash
terraform destroy
```

## Architecture

- **VPC**: 2 public subnets + 2 private subnets across 2 AZs
- **ECS Fargate**: Tasks run in private subnets (no public IP)
- **ALB**: Application Load Balancer in public subnets
- **NAT Gateway**: Allows private subnet egress for image pulls

## API Response

```json
{
  "timestamp": "2024-01-15T10:30:00Z",
  "ip": "203.0.113.50"
}
```
