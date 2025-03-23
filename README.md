# Ansible K3s Rancher Project

This project automates the deployment of K3s (Lightweight Kubernetes) on a cluster of worker nodes using Ansible. The playbooks provided handle various stages of setting up and managing the K3s cluster.

## Project Overview

I use my Terraform project to instantiate a 5-node cluster on a cost-effective mini-PC with the following specs:
- **CPU**: Ryzen 7 5700
- **RAM**: 32GB 3200-SODIMM
- **Storage**: 512GB NVMe Disk

### Node Configuration

- **1 Control Node**: 8 CPU / 8GB RAM / 50GB Disk / 2GB Swap
- **4 Worker Nodes**: 8 CPU / 4GB RAM / 50GB Disk / 2GB Swap

Find the Terraform project here: [Terraform Ansible Lab](https://github.com/abaas-madscience/terraform-ansible-lab)

## Project Structure

- `inventory/hosts.yml`: Inventory file defining the hosts in the cluster.
- `playbooks/001-setup-control.yml`: Playbook to set up the control node.
- `playbooks/002-setup-workers.yml`: Playbook to set up the worker nodes.
- `playbooks/003-join-workers.yml`: Playbook to join worker nodes to the K3s cluster.
- `playbooks/004-install-metallb.yml`: Playbook to install MetalLB Load Balancer.
- `playbooks/004-install-rancher.yml`: Playbook to install Rancher on the control node.
- `playbooks/005-configure-rancher.yml`: Playbook to configure Rancher.
- `playbooks/006-install-victoriametrics.yml`: Playbook to install VictoriaMetrics.
- `playbooks/007-install-grafana.yml`: Playbook to install Grafana.
- `playbooks/008-install-loki.yml`: Playbook to install Loki (Work in progress).
- `playbooks/roles/k3s_worker/tasks/main.yml`: Role tasks for installing K3s on worker nodes.

## Playbooks

### 001-setup-control.yml

Sets up the control node for the K3s cluster.

### 002-setup-workers.yml

Sets up the worker nodes for the K3s cluster.

### 003-join-workers.yml

Joins worker nodes to the K3s cluster. It consists of two main plays:
1. **Load K3s Join Token**: Reads and sets the K3s join token.
2. **Install K3s on Workers**: Installs the K3s agent on worker nodes.

### 004-install-metallb.yml

Installs MetalLB Load Balancer.

### 004-install-rancher.yml

Installs Rancher on the control node. Bonus: Displays the URL and initial password.

### 005-configure-rancher.yml

Configures Rancher after installation.

### 006-install-victoriametrics.yml

Installs the lightweight VictoriaMetrics.

### 008-install-loki.yml

Installs Loki (Work in progress).

## Usage

To run the playbooks, use the following command:

```sh
ansible-playbook -i inventory/hosts.yml playbooks/<playbook-name>.yml
```

Ensure that the `inventory/hosts.yml` file is correctly configured with your control and worker nodes.

## Inventory File

The `inventory/hosts.yml` file should define your control and worker nodes. Here is an example structure:

```yaml
all:
  hosts:
    control:
      ansible_host: control.lab.local
    worker1:
      ansible_host: worker1.lab.local
    worker2:
      ansible_host: worker2.lab.local
  children:
    control:
      hosts:
        control:
    workers:
      hosts:
        worker1:
        worker2:
```

## Conclusion

This project provides a streamlined way to deploy and manage a K3s cluster using Ansible. By following the structure and instructions provided, you can easily set up, configure, and maintain your K3s cluster.
