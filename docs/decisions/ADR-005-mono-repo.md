# ADR-005 — Why Mono-Repo Structure

## Status
Accepted

## Context
The project contains Terraform code, Kubernetes manifests, GitHub Actions workflows, OPA policies, monitoring configuration, and Python scripts. These can be organised in a single repository or multiple separate repositories.

## Decision
Use a single mono-repository containing all project components.

## Reasons
- Infrastructure code and application manifests are tightly coupled in this project
- Single source of truth — one Git history for all changes
- Simpler ArgoCD configuration — one repository to watch
- Easier for a single engineer to manage during development
- Changes can be traced from infrastructure through to deployment in one place

## Trade-offs
- In larger organisations, separate repositories enforce stronger ownership boundaries
- Mono-repo can become harder to navigate as the project grows

## Future Consideration
Split into separate repositories (infrastructure, application, platform-config) if the project scales or multiple contributors are involved.
