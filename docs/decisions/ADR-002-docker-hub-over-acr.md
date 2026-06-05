# ADR-002 — Why Docker Hub Instead of Azure Container Registry

## Status
Accepted — Phase 1 only

## Context
A container image registry is required to store Docker images built by GitHub Actions and pulled by ArgoCD for deployment to AKS.

## Decision
Use Docker Hub as the container registry for Phase 1.

## Reasons
- Works across both AWS and Azure without additional Terraform configuration
- Demonstrates a registry-agnostic GitOps workflow — ArgoCD and AKS work identically regardless of registry
- Intentionally decouples registry choice from Kubernetes platform choice
- Free tier sufficient for portfolio use

## Trade-offs
- Less cloud-native than Azure Container Registry (ACR)
- Docker Hub has pull rate limits on free tier
- Not integrated with Azure Managed Identity

## Future Enhancement
Replace Docker Hub with Azure Container Registry (ACR) in Phase 2 to demonstrate cloud-native registry integration with Managed Identity authentication.
