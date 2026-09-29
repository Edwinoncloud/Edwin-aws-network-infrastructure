# AWS Network Infrastructure with Terraform

## Overview

This project deploys a segmented AWS network using Terraform. I first built and tested the infrastructure manually in AWS, then recreated it using Infrastructure as Code.

The architecture separates public and private resources while allowing private resources to access the internet through a NAT Gateway.

## Architecture

![AWS Network Architecture](images/Architecture.png)

### Routing

**Public subnet**

`0.0.0.0/0 → Internet Gateway`

**Private subnet**

`0.0.0.0/0 → NAT Gateway`

The NAT Gateway resides in the public subnet and provides outbound internet access for the private EC2 instance without assigning the private instance a public IPv4 address.

Traffic between the public and private subnets uses the VPC's local route.

## Security

The public EC2 Security Group allows:

- SSH (TCP/22) from a specified administrator IP
- HTTP (TCP/80) from the internet

The private EC2 Security Group allows:

- SSH (TCP/22) from resources using the public EC2 Security Group
- Outbound traffic through the NAT Gateway

The SSH source CIDR is supplied to Terraform as a variable instead of being hard-coded.

## Testing

I validated the deployment by:

- Connecting from my local machine to the public EC2 instance using SSH
- Connecting from the public EC2 instance to the private EC2 instance
- Confirming the private EC2 instance had no public IPv4 address
- Testing outbound internet connectivity from the private instance
- Confirming the private instance's outbound traffic used the NAT Gateway's Elastic IP

## Technologies

- AWS
- Terraform
- Amazon VPC
- Amazon EC2
- NAT Gateway
- Internet Gateway
- Security Groups
- Linux
- Git/GitHub

## Terraform Workflow

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform destroy
```

Sensitive Terraform state, variable files, AWS credentials, and SSH private keys are excluded from version control.