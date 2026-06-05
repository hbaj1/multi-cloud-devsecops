# Multi-Cloud DevSecOps Platform

A production-style portfolio project demonstrating multi-cloud infrastructure, GitOps, DevSecOps, monitoring, and Python automation across AWS and Azure.

---

## Project Overview

This platform deploys a containerised 3-tier application on Azure Kubernetes Service (AKS) using a fully automated CI/CD pipeline, GitOps workflow, and integrated security scanning — while demonstrating multi-cloud infrastructure management across AWS and Azure using Terraform.

> **Note:** This project demonstrates multi-cloud infrastructure management — Terraform, networking, state management, and automation across AWS and Azure — rather than multi-cloud workload deployment.

---

## Architecture

```
GitHub Repository
       |
GitHub Actions CI/CD
       |
  ┌────┴────┐
  │         │
Infrastructure    Application
Pipeline          Pipeline
  │         │
Checkov+OPA  Docker Build
  │         │
Terraform    Trivy Scan
  │         │
  ├─────────┘
  │
  ├── AWS (eu-west-2)              Azure (UK South)
  │   ├── VPC                      ├── VNet
  │   ├── Public Subnet            ├── AKS Subnet
  │   │   └── EC2 Bastion          ├── Monitoring Subnet
  │   ├── Private Subnet           │   └── Monitoring VM
  │   │   └── EC2 Admin VM         │       ├── Prometheus
  │   └── S3 + DynamoDB            │       └── Grafana
  │       (Terraform State)        └── AKS Cluster
  │                                    ├── ArgoCD
  │                                    └── Docker Voting App
  │
  └── Python Automation
      ├── AWS Inventory (boto3)
      └── Azure Inventory (Azure SDK)
```

---

## Technologies

| Category | Tools |
|---|---|
| Cloud | AWS, Azure |
| Infrastructure as Code | Terraform |
| CI/CD | GitHub Actions |
| Container Orchestration | Kubernetes (AKS) |
| GitOps | ArgoCD |
| Security Scanning | Checkov, Trivy, OPA |
| Monitoring | Prometheus, Grafana |
| Automation | Python, boto3, Azure SDK |
| Container Registry | Docker Hub |
| Source Control | Git, GitHub |

---

## Repository Structure

```
multi-cloud-devsecops/
├── terraform/
│   ├── aws/              # AWS root module
│   ├── azure/            # Azure root module
│   ├── modules/
│   │   ├── aws/          # AWS reusable modules
│   │   └── azure/        # Azure reusable modules
│   └── environments/
│       ├── dev/
│       ├── staging/
│       └── prod/
├── k8s/
│   ├── voting-app/       # Application manifests
│   ├── argocd/           # ArgoCD application definitions
│   ├── rbac/             # Kubernetes RBAC
│   └── network-policies/ # Pod network policies
├── .github/
│   └── workflows/        # GitHub Actions pipelines
├── opa/
│   └── policies/         # OPA Rego policies
├── monitoring/
│   ├── prometheus/       # Prometheus configuration
│   └── grafana/          # Grafana dashboards
├── python/
│   ├── aws/              # AWS compliance scripts
│   └── azure/            # Azure compliance scripts
└── docs/
    └── decisions/        # Architecture Decision Records
```

---

## Design Decisions

Key architectural decisions are documented in [/docs/decisions](/docs/decisions):

| ADR | Decision |
|---|---|
| [ADR-001](docs/decisions/ADR-001-aks-over-eks.md) | Why AKS instead of EKS |
| [ADR-002](docs/decisions/ADR-002-docker-hub-over-acr.md) | Why Docker Hub instead of ACR |
| [ADR-003](docs/decisions/ADR-003-vm-monitoring.md) | Why Prometheus on VM instead of in-cluster |
| [ADR-004](docs/decisions/ADR-004-separate-terraform-backends.md) | Why separate Terraform backends |
| [ADR-005](docs/decisions/ADR-005-mono-repo.md) | Why mono-repo structure |

---

## CI/CD Pipeline

### Infrastructure Pipeline
```
Git Push → Terraform Format → Terraform Validate → Checkov → OPA → Terraform Plan → Manual Approval → Terraform Apply
```

### Application Pipeline
```
Git Push → Docker Build → Trivy Scan → Push to Docker Hub → Update Manifests → ArgoCD Sync → Deploy to AKS
```

---

## Network Design

| Cloud | Resource | CIDR |
|---|---|---|
| AWS | VPC | 10.100.0.0/16 |
| AWS | Public Subnet | 10.100.1.0/24 |
| AWS | Private Subnet | 10.100.2.0/24 |
| Azure | VNet | 10.0.0.0/16 |
| Azure | AKS Subnet | 10.0.2.0/23 |
| Azure | Monitoring Subnet | 10.0.4.0/24 |

---

## Prerequisites

- AWS account with an IAM user with sufficient permissions to create project resources
- Azure account with active subscription
- Tools: Terraform >= 1.8, AWS CLI, Azure CLI, Git, kubectl

---

## Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/hbaj1/multi-cloud-devsecops.git
cd multi-cloud-devsecops
```

### 2. Configure AWS credentials
```bash
aws configure
# Region: eu-west-2
```

### 3. Configure Azure credentials
```bash
az login
az account set --subscription "<your-subscription-id>"
```

### 4. Create Terraform state backend (AWS)
```bash
# Create S3 bucket and DynamoDB table manually before terraform init
# See terraform/aws/backend.tf for bucket and table names
```

### 5. Deploy AWS infrastructure
```bash
cd terraform/aws
cp ../../environments/dev/terraform.tfvars .
terraform init
terraform plan
terraform apply
```

### 6. Deploy Azure infrastructure
```bash
cd terraform/azure
terraform init
terraform plan
terraform apply
```

---

## Screenshots

*Coming during implementation:*

- [ ] AWS VPC and subnet layout
- [ ] EC2 Bastion and Admin VM
- [ ] AKS cluster and node pools
- [ ] GitHub Actions pipeline runs
- [ ] ArgoCD sync status
- [ ] Grafana dashboard
- [ ] Python compliance report output

---

## Project Status

- [ ] Phase 1 — AWS Foundation (VPC, Bastion, Admin VM, S3 state)
- [ ] Phase 2 — Azure + CI/CD (VNet, AKS, Monitoring VM, GitHub Actions)
- [ ] Phase 3 — GitOps Deployment (ArgoCD, Docker Voting App)
- [ ] Phase 4 — Monitoring + Security (Prometheus, Grafana, Checkov, Trivy, OPA, Python)

---

## Assumptions and Limitations

- Built for learning and portfolio demonstration
- Dev environment only — staging and prod follow the same pattern
- Cost optimised for cloud free credits
- Phase 1 uses long-lived credentials — OIDC planned as future enhancement
- Monitoring VM is a single point of failure — accepted trade-off for this scope
- No centralised logging in Phase 1

---

## Author

**Hepzhiba Abraham**
AWS Certified Solutions Architect | HashiCorp Terraform Associate 
[LinkedIn](https://linkedin.com/in/hepzhiba-abraham) | [GitHub](https://github.com/hbaj1)
