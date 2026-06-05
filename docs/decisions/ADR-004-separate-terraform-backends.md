# ADR-004 — Why Separate Terraform Backends for AWS and Azure

## Status
Accepted

## Context
Terraform requires a remote state backend to store infrastructure state. Options include a single shared backend or separate backends per cloud.

## Decision
Use separate Terraform backends — S3 + DynamoDB for AWS and Azure Storage Account for Azure.

## Reasons
- AWS and Azure are maintained as separate root modules with independent lifecycles
- Demonstrates cloud-specific state management patterns used in enterprise environments
- Reduces blast radius — a Terraform error in one cloud cannot affect the other
- Matches how multi-cloud teams typically operate in production

## Trade-offs
- Two backends to configure and maintain instead of one
- Slightly more setup complexity at the start

## Alternative Considered
Single backend (Terraform Cloud or one S3 bucket for both). Rejected because the multi-cloud state pattern provides more interview and learning value.
