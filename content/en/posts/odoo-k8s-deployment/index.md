---
title: "Odoo ERP on Kubernetes"
date: 2025-03-15
demo_url: "https://odoo.app.r2b001.site"
repo_url: "https://github.com/bensalah-nabil/proxmox-ubuntu-k8s/tree/main/helm/odoo"
---

## Project Overview
Production deployment of Odoo ERP on Kubernetes with:
- High availability (multi-AZ)
- Auto-scaling (2-10 pods)
- Automated backups
- GitOps workflow

## Access
{{ if .Params.demo_url }}▶ [Live Demo]({{ .Params.demo_url }}){{ end }}
{{ if .Params.repo_url }}▶ [Source Code]({{ .Params.repo_url }}){{ end }}

## Architecture
```mermaid
graph TD
    A[Users] --> B[Cloudflare]
    B --> C[AWS ALB]
    C --> D[Odoo Pods]
    D --> E[PostgreSQL]
    E --> F[EFS Storage]
