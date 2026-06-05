# ADR-003 — Why Prometheus on VM Instead of In-Cluster Monitoring

## Status
Accepted

## Context
The platform requires metrics collection and dashboarding for AKS cluster health, node metrics, and application availability.

## Decision
Deploy Prometheus and Grafana on a dedicated Azure VM in a private monitoring subnet rather than inside the AKS cluster.

## Reasons
- Demonstrates Azure VM provisioning and Linux administration skills
- Keeps monitoring isolated from the application cluster
- Simpler operational model — easier to understand and troubleshoot during learning
- Prometheus authenticates to AKS via a read-only Kubernetes Service Account and kubeconfig

## Trade-offs
- Monitoring VM is a single point of failure
- Not the current best practice for Kubernetes-native monitoring
- Adds VM management overhead

## Production Preference
Deploy kube-prometheus-stack inside AKS or use Azure Managed Prometheus for high availability and tighter cluster integration.
