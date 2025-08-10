---
title: "Odoo ERP Deployment on Kubernetes"
date: 2025-03-15
tech_stack: 
  - "Kubernetes"
  - "Helm"
  - "PostgreSQL"
  - "AWS EKS"
  - "Terraform"
  - "ArgoCD"
status: "Production"
demo_url: "https://odoo.app.r2b001.site"
repo_url: "https://github.com/bensalah-nabil/proxmox-ubuntu-k8s/tree/main/helm/odoo"
architecture_diagram: "odoo-architecture.png"
metrics:
  - "99.95% Uptime"
  - "50+ concurrent users"
  - "Auto-scaling from 2 to 10 pods"
features:
  - "Highly available deployment"
  - "CI/CD pipeline"
  - "Automated backups"
  - "Monitoring with Prometheus/Grafana"
screenshots:
  - "odoo-dashboard.png"
  - "k8s-dashboard.png"
---

## Project Overview
Production-grade Odoo ERP deployment on Kubernetes with high availability and automated scaling.

## Technical Implementation

```mermaid
graph TD
    A[Users] --> B[Cloudflare]
    B --> C[AWS ALB]
    C --> D[Odoo Pods]
    D --> E[PostgreSQL Cluster]
    E --> F[EFS Storage]
    D --> G[Redis Cache]
