+++
date = '2025-08-11T08:05:34Z'
draft = false
title = 'Deploying Odoo on Kubernetes with Bitnami Helm & External PostgreSQL (on Proxmox)'
tags = ['odoo', 'kubernetes', 'helm', 'bitnami', 'postgresql', 'proxmox']
+++

## Introduction

In this post, I’ll walk you through how I deployed **Odoo** using the **Bitnami Helm chart** on my **on-premises Kubernetes cluster** hosted on **Proxmox**, while connecting it to an **external PostgreSQL database** running on a separate Proxmox VM.  

---

## Architecture Overview

```mermaid
flowchart LR
    subgraph Proxmox
        subgraph K8s_Cluster[Kubernetes Cluster]
            CP[Control Plane Node<br>vm-k8s-dev-01-cp-01]
            WK[Worker Node<br>vm-k8s-dev-01-wk-01]
            subgraph Odoo_Deployment[Odoo Deployment (Bitnami Helm)]
                Odoo1[Odoo Pod 1]
                Odoo2[Odoo Pod 2]
                PVC[Persistent Volume<br>/bitnami/odoo]
            end
        end
        DBVM[PostgreSQL VM<br>vm-pgsql-dev-01]
    end

    Odoo1 --- PVC
    Odoo2 --- PVC
    Odoo1 --- DBVM
    Odoo2 --- DBVM
    CP --- WK
````

---

## My Environment

* **Proxmox Cluster**:

  * `vm-k8s-dev-01-cp-01`: Kubernetes control-plane node (Ubuntu 22.04)
  * `vm-k8s-dev-01-wk-01`: Kubernetes worker node
  * `vm-pgsql-dev-01`: PostgreSQL VM
* **Kubernetes**:

  * Version: `1.30.x`
  * Networking: Calico CNI
* **PostgreSQL**:

  * Version: `15.x`
  * Hosted on dedicated Proxmox VM
* **Helm**:

  * Version: `v3.15.x`
* **Bitnami Odoo Chart**:

  * Chart: `odoo`
  * Source: [https://bitnami.com/stack/odoo/helm](https://bitnami.com/stack/odoo/helm)

---

## Step 1 — Preparing PostgreSQL

On my PostgreSQL VM (`vm-pgsql-dev-01`):

```bash
sudo -u postgres psql
```

Create the Odoo database and user:

```sql
CREATE DATABASE odoo_db;
CREATE USER odoo_user WITH PASSWORD 'SuperSecretPassword';
GRANT ALL PRIVILEGES ON DATABASE odoo_db TO odoo_user;
```

Allow connections from Kubernetes nodes in `pg_hba.conf`:

```conf
# Example: allow all K8s subnet (adjust CIDR)
host    odoo_db     odoo_user     10.244.0.0/16     md5
```

In `postgresql.conf`, listen on all interfaces:

```conf
listen_addresses = '*'
```

Restart PostgreSQL:

```bash
sudo systemctl restart postgresql
```

Test from a Kubernetes node:

```bash
psql -h vm-pgsql-dev-01 -U odoo_user -d odoo_db
```

---

## Step 2 — Creating a Kubernetes Namespace

```bash
kubectl create namespace odoo
```

---

## Step 3 — Adding the Bitnami Helm Repository

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

---

## Step 4 — Creating the `values.yaml`

```yaml
# values.yaml
image:
  repository: bitnami/odoo
  tag: 17-debian-12
  pullPolicy: IfNotPresent

postgresql:
  enabled: false

externalDatabase:
  host: vm-pgsql-dev-01
  port: 5432
  user: odoo_user
  password: SuperSecretPassword
  database: odoo_db

service:
  type: LoadBalancer
  port: 80

persistence:
  enabled: true
  size: 10Gi
  storageClass: nfs-client # adjust for your storage provisioner

ingress:
  enabled: true
  hostname: odoo.local
  ingressClassName: nginx
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
  tls: []
```

---

## Step 5 — Deploying Odoo

```bash
helm install odoo bitnami/odoo -f values.yaml -n odoo
```

Check:

```bash
kubectl get pods -n odoo
kubectl get svc -n odoo
```

---

## Step 6 — Accessing Odoo

If using LoadBalancer (MetalLB):

```bash
kubectl get svc -n odoo
```

Open the assigned IP in your browser.
If using Ingress, make sure DNS or `/etc/hosts` points `odoo.local` to your ingress controller.

---

## Step 7 — Persistent Volumes & Backups

The Bitnami chart stores uploaded files and modules in `/bitnami/odoo` (persistent).

For PostgreSQL backups:

```bash
pg_dump -h vm-pgsql-dev-01 -U odoo_user -d odoo_db > odoo_backup.sql
```

---

## Step 8 — Scaling & Updates

Scale app layer:

```bash
kubectl scale deployment odoo -n odoo --replicas=3
```

Update Odoo:

```bash
helm upgrade odoo bitnami/odoo -f values.yaml -n odoo
```

---

## Conclusion

Separating Odoo and PostgreSQL provides:

* Independent DB maintenance
* Easier backups
* Safer upgrades

Running both on **Proxmox** keeps full control in your hands while staying on-premises.

---

**References**:

* [Bitnami Odoo Helm Chart](https://github.com/bitnami/charts/tree/main/bitnami/odoo)
* [Odoo Documentation](https://www.odoo.com/documentation)
* [PostgreSQL Docs](https://www.postgresql.org/docs/)


