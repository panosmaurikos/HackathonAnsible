# Kubernetes Cluster Ansible Playbook

Ansible playbook για εγκατάσταση **K3s** ή **MicroK8s** με σωστό CNI (Calico).

## Features

- ✅ Επιλογή μεταξύ K3s και MicroK8s
- ✅ Calico CNI για σωστό networking
- ✅ `kubectl` λειτουργεί χωρίς prefix (και στα δύο!)
- ✅ Kubeconfig setup για user και root
- ✅ kubectl bash completion & aliases
- ✅ Multi-node cluster support

## Quick Start

### 1. Προετοιμασία Inventory

Επεξεργάσου το `inventory/hosts.yml`:

```yaml
all:
  children:
    k8s_nodes:
      children:
        masters:
          hosts:
            master1:
              ansible_host: 192.168.1.100
              node_role: master
        workers:
          hosts:
            worker1:
              ansible_host: 192.168.1.101
              node_role: worker
```

### 2. Εκτέλεση

**Με prompt για επιλογή distribution:**
```bash
ansible-playbook site.yml
```

**Απευθείας K3s:**
```bash
ansible-playbook site.yml -e "k8s_distribution=k3s"
```

**Απευθείας MicroK8s:**
```bash
ansible-playbook site.yml -e "k8s_distribution=microk8s"
```

## Configuration Options

Επεξεργάσου το `group_vars/all.yml` για customization:

### K3s Options
```yaml
k3s_channel: "stable"           # stable, latest
k3s_disable_components:
  - traefik                     # disable traefik
k3s_flannel_backend: "none"     # disable flannel, use Calico
```

### MicroK8s Options
```yaml
microk8s_channel: "1.29"
microk8s_use_calico: true       # use Calico instead of default CNI
microk8s_addons:
  - dns
  - storage
  - helm3
```

### CNI Options
```yaml
cni_plugin: "calico"            # calico or cilium
calico_version: "v3.27.0"
pod_cidr: "10.42.0.0/16"
```

## After Installation

```bash
# Verify cluster
kubectl get nodes
kubectl get pods -A

# Check CNI
kubectl get pods -n calico-system

# Useful aliases (auto-installed)
kgpa   # kubectl get pods -A
kgn    # kubectl get nodes
kga    # kubectl get all
```

## Project Structure

```
k8s-ansible/
├── ansible.cfg              # Ansible configuration
├── site.yml                 # Main playbook
├── inventory/
│   └── hosts.yml            # Inventory file
├── group_vars/
│   └── all.yml              # Global variables
└── roles/
    ├── common/              # Prerequisites
    │   ├── tasks/main.yml
    │   └── handlers/main.yml
    ├── k3s/                 # K3s installation
    │   ├── tasks/main.yml
    │   ├── handlers/main.yml
    │   └── defaults/main.yml
    ├── microk8s/            # MicroK8s installation
    │   ├── tasks/main.yml
    │   ├── handlers/main.yml
    │   └── defaults/main.yml
    └── cni/                 # CNI (Calico/Cilium)
        ├── tasks/main.yml
        ├── templates/
        │   └── calico-custom-resources.yaml.j2
        └── defaults/main.yml
```

## Differences: K3s vs MicroK8s

| Feature | K3s | MicroK8s |
|---------|-----|----------|
| Installation | Script | Snap |
| CNI | External (Calico) | Addon-based |
| Resource Usage | Lower | Higher |
| Best For | Edge, IoT, ARM | Development, Ubuntu |
| Multi-node | Built-in | Via `microk8s add-node` |

## Troubleshooting

### kubectl permission denied (MicroK8s)
```bash
# Logout and login or run:
newgrp microk8s
```

### Nodes NotReady
```bash
# Check CNI pods
kubectl get pods -n calico-system
kubectl describe pod -n calico-system <pod-name>
```

### Reset cluster

**K3s:**
```bash
/usr/local/bin/k3s-uninstall.sh        # master
/usr/local/bin/k3s-agent-uninstall.sh  # worker
```

**MicroK8s:**
```bash
sudo snap remove microk8s --purge
```

## Requirements

- Ubuntu 20.04+ / Debian 11+
- Ansible 2.12+
- Python 3.8+
- SSH access to target nodes
- sudo privileges

## License

MIT
