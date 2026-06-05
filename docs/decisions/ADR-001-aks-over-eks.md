# ADR-001 — Why AKS Instead of EKS

## Status
Accepted

## Context
The project requires a Kubernetes platform to host the Docker Voting App with GitOps-based deployments via ArgoCD. Both AKS (Azure) and EKS (AWS) are valid options.

## Decision
Use AKS on Azure as the sole Kubernetes platform for this project.

## Reasons
- Demonstrates Azure Kubernetes expertise alongside AWS infrastructure skills
- AKS control plane is free — reduces cost for a portfolio project
- Avoids the complexity and cost of managing two Kubernetes clusters simultaneously
- AWS side demonstrates networking, state management, and administration patterns without EKS

## Trade-offs
- AWS Kubernetes experience not demonstrated in this version
- Multi-cloud workload deployment not achieved — only multi-cloud infrastructure management

## Future Enhancement
Evaluate adding EKS as a Phase 2 component once AKS is stable and well understood.
