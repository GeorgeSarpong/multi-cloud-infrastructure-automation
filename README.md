# Multi-Cloud Infrastructure Automation & Deployment

![AWS](https://img.shields.io/badge/AWS-Solutions%20Architect%20Pro-orange)
![Azure](https://img.shields.io/badge/Azure-Solutions%20Architect%20Expert-blue)
![Terraform](https://img.shields.io/badge/Terraform-Associate-purple)
![Kubernetes](https://img.shields.io/badge/Kubernetes-CKA-blue)
![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-black)
![Docker](https://img.shields.io/badge/Docker-Containerization-blue)

---

## Project Overview

This project deploys and documents scalable multi-cloud infrastructure across AWS and Azure following infrastructure as code best practices with full CI/CD integration, security controls, and observability stack.

Built to demonstrate operational readiness for:
- ✅ Cloud Infrastructure Engineer roles
- ✅ Cloud & DevOps Engineer roles
- ✅ Site Reliability Engineer roles
- ✅ Platform Engineer roles

---

## Architecture Overview
---

## Repository Structure

---

## 🚀 Infrastructure Components

### AWS Components
| Component | Service | Purpose |
|---|---|---|
| Networking | VPC, Subnets, Route Tables | Network isolation and routing |
| Compute | EC2, Auto Scaling, EKS | Application hosting and orchestration |
| Database | RDS Multi-AZ | High availability database |
| Storage | S3, EBS | Object and block storage |
| Security | IAM, Security Groups, KMS | Access control and encryption |
| DNS | Route 53 | DNS management and failover |
| Monitoring | CloudWatch, SNS | Monitoring and alerting |

### Azure Components
| Component | Service | Purpose |
|---|---|---|
| Networking | VNet, NSG, Load Balancer | Network and traffic management |
| Compute | VMs, AKS, Scale Sets | Application hosting |
| Database | Azure SQL, Cosmos DB | Database services |
| Storage | Blob Storage, Azure Files | Storage services |
| Security | Azure AD, Key Vault | Identity and secrets management |
| Monitoring | Azure Monitor, Log Analytics | Monitoring and logging |

---

## 🔧 Terraform Configuration — AWS Example

```hcl
# AWS VPC Configuration
resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name        = "${var.environment}-vpc"
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}

# Public Subnet
resource "aws_subnet" "public" {
  count             = length(var.availability_zones)
  vpc_id            = aws_vpc.main.id
  cidr_block        = var.public_subnet_cidrs[count.index]
  availability_zone = var.availability_zones[count.index]

  map_public_ip_on_launch = true

  tags = {
    Name = "${var.environment}-public-subnet-${count.index + 1}"
    Type = "Public"
  }
}

# EC2 Instance
resource "aws_instance" "app_server" {
  ami           = var.ami_id
  instance_type = var.instance_type
  subnet_id     = aws_subnet.public[0].id

  vpc_security_group_ids = [aws_security_group.app.id]
  iam_instance_profile   = aws_iam_instance_profile.app.name

  tags = {
    Name        = "${var.environment}-app-server"
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}
```

---

## 🔄 CI/CD Pipeline — GitHub Actions

```yaml
name: Infrastructure Deployment Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  terraform-validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: 1.5.0

      - name: Terraform Init
        run: terraform init

      - name: Terraform Validate
        run: terraform validate

      - name: Terraform Plan
        run: terraform plan
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}

  terraform-apply:
    needs: terraform-validate
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3

      - name: Terraform Apply
        run: terraform apply -auto-approve
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

---

## Security Controls

| Control | Implementation | Standard |
|---|---|---|
| Identity & Access | IAM roles with least privilege | AWS IAM / Azure RBAC |
| Network Security | Security groups and NACLs | CIS Benchmarks |
| Data Encryption | KMS encryption at rest and in transit | SOC2 / NIST |
| Secrets Management | AWS Secrets Manager / Azure Key Vault | NIST SP 800-57 |
| Compliance Monitoring | AWS Config / Azure Policy | SOC2 / GDPR |
| Vulnerability Scanning | AWS Inspector / Defender for Cloud | NIST CSF |

---

## Cost Optimisation — FinOps

| Strategy | Implementation | Estimated Saving |
|---|---|---|
| Right-sizing | Instance type optimisation | 20–30% |
| Reserved Instances | 1-year commitments for stable workloads | 30–40% |
| Auto Scaling | Scale down during off-peak hours | 15–25% |
| S3 Lifecycle Policies | Move old data to cheaper storage tiers | 40–60% |
| Unused Resource Cleanup | Automated detection and removal | 10–15% |

---

## Deployment Runbook

### Pre-Deployment Checklist
- [ ] Code review completed and approved
- [ ] Terraform plan reviewed and approved
- [ ] Change request raised and approved
- [ ] Rollback procedure confirmed
- [ ] Stakeholders notified of deployment window
- [ ] Monitoring dashboards open and ready

### Deployment Steps
1. Merge approved pull request to main branch
2. CI/CD pipeline triggers automatically
3. Terraform init and validate runs
4. Terraform plan output reviewed
5. Approval gate — manual approval required for apply
6. Terraform apply executes
7. Post-deployment validation tests run
8. Monitoring dashboards verified
9. Stakeholders notified of successful deployment

### Post-Deployment Validation
- Verify all resources created successfully
- Check monitoring dashboards for anomalies
- Run smoke tests against deployed infrastructure
- Verify security group rules are correct
- Confirm DNS resolution working
- Check CloudWatch logs for errors

---

## Standards & Frameworks Referenced

- **HashiCorp Terraform** — Infrastructure as Code
- **AWS Well-Architected Framework** — Cloud architecture best practices
- **Microsoft Azure Architecture Framework** — Azure best practices
- **CIS Benchmarks** — Security configuration standards
- **SOC2** — Security and availability controls
- **NIST CSF** — Cybersecurity framework

---

## Tools & Technologies

![Terraform](https://img.shields.io/badge/Terraform-1.5-purple)
![AWS](https://img.shields.io/badge/AWS-Multi%20Service-orange)
![Azure](https://img.shields.io/badge/Azure-Multi%20Service-blue)
![Docker](https://img.shields.io/badge/Docker-Containerization-blue)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-blue)
![GitHub Actions](https://img.shields.io/badge/GitHub-Actions-black)
![Jenkins](https://img.shields.io/badge/Jenkins-Pipeline-red)
![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-orange)
![Grafana](https://img.shields.io/badge/Grafana-Dashboards-orange)

---

## Author

**George Amankwaa Sarpong**
Cloud Infrastructure Engineer | DevOps | Multi-Cloud Architecture
📍 Accra, Ghana 
🔗 [LinkedIn](https://linkedin.com/in/georgesarpong)
🌐 [GitHub Portfolio](https://github.com/GeorgeSarpong)

---

*This project is part of a broader portfolio demonstrating readiness for Cloud Infrastructure Engineer and Cloud DevOps Engineer roles in the US and Global market.*
