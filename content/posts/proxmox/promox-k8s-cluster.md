+++
date = '2025-09-01T15:08:47Z'
draft = true
title = 'Promox K8s Cluster'
+++
Today I could say that I just completed building my own kubernetes cluster from scratch , so i decided to document all steps and operations i have do to accomplish this

### Introduction
### Features

    Cilium: Networking and L2 load balancer.
    Ingress Nginx: Ingress controller.
    MetaLB: Metalb
    Longhorn: Distributed storage.

### Prerequisites

    Proxmox VE cluster/standalone server.
    A linux VM which acts as a workstation/bastion. In this guide, we use Debian 11.
    Basic understanding of Linux, Containers and Kubernetes.

### Prepare the workstation 
1. On your linux workstation install the tools required for this guide. Let us start off with Terraform. 

bash'''
sudo apt update
sudo apt install terraform
terraform -v
'''
2. Clone my github repository https://github.com/bensalah-nabil/proxmox-ubuntu-k8s

bash'''
cd $HOME
git clone https://github.com/c0depool/c0depool-iac.git
'''

### System requirements

Before we start the deployment in Proxmox VE, let’s ensure we have the following components in place:

    Internal network
    VM template
    SSH key pair
    Bastion host

In the next section, I will provide a step-by-step procedure on how to configure each of the required components.

### Installing required components

