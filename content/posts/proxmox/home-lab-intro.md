+++
date = '2025-09-03T15:08:47Z'
draft = false
title = 'Building My Homelab – Why and How'
+++

## 🏠 What is a Homelab?

A **homelab** is a personal environment where you can experiment with technologies, test architectures, and learn without risking production systems.
Think of it as your own **mini datacenter at home**, running on whatever resources you can gather — an old server, a few virtual machines, or even cloud credits.

For IT professionals, engineers, and enthusiasts, a homelab is the perfect way to:

- Learn new technologies by **breaking and fixing them**.
- Test tools and workflows before bringing them to production.
- Simulate enterprise-like setups (firewalls, load balancers, Kubernetes, monitoring, etc.).
- Build skills in networking, DevOps, and cloud-native practices.

---

## 💡 Why I Built My Homelab

I wanted a place where I could:

1. **Experiment safely** → Without worrying about downtime or breaking production.
2. **Learn modern technologies** → Kubernetes, Terraform, CI/CD, observability stacks.
3. **Recreate real-world scenarios** → Like firewalls, HAProxy load balancing, Active Directory, SOC tooling.
4. **Centralize everything I’ve learned** → Into a structured environment I can continuously improve.

This series is my attempt to document and share that journey.

---

## 🖼️ My Homelab Architecture

Here’s the high-level architecture of my current setup:

{{< figure src="/proxmox/homelab-architecture.png" alt="Homelab Architecture Diagram" caption="Global architecture of my homelab on Proxmox VE" >}}

- **Proxmox VE** is the virtualization layer.
- **pfSense** acts as a firewall and router.
- **HAProxy** handles load balancing.
- Multiple **networks** separate workloads (WAN, LAN, Kubernetes, AD).
- Dedicated zones for:
  - **Workstations / Bastion Hosts**
  - **Kubernetes Cluster**
  - **Databases**
  - **Active Directory (planned)**
  - **SOC Tools (monitoring, security)**

---

## 🚀 What This Blog Series Will Cover

Instead of jumping directly into Kubernetes, I’ll build up piece by piece:

1. **What is a Homelab? (this post)**
2. Setting up **Proxmox VE** and base networking.
3. Configuring **pfSense** as the firewall/router.
4. Building a secure **Workstation / Bastion Host**.
5. Introducing **HAProxy** for load balancing.
6. Deploying a **Kubernetes Cluster** with modern networking and storage.
7. Adding **Databases** and other backend services.
8. Integrating **Monitoring, Logging, and Security tools**.
9. Future plans → AD, SOC automation, hybrid cloud.

---

## 🔮 Next Step

In the next post, I’ll show how I set up the **Proxmox VE environment** and prepared the foundation for the entire homelab.
This includes networking, iptables/NAT, and the first VMs that serve as gateways into the cluster.

Stay tuned!
