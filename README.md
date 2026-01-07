# Kubernetes Cluster Ansible Playbook





```yaml
all:
  children:
    k8s_nodes:
      children:
        masters:
          hosts:
            master1:
              ansible_host: 192.168.1.100
```

```bash
ansible-playbook site.yml
```

```bash
ansible-playbook site.yml -e "k8s_distribution=k3s"
```

```bash
ansible-playbook site.yml -e "k8s_distribution=microk8s"
```

```
┌─────────────────────────────────────┐
│           KServe                    │  ← ML Model Serving
├─────────────────────────────────────┤
│       Knative Serving               │  ← Serverless
├─────────────────────────────────────┤
│     Kourier ή Istio                 │  ← HTTP Routing
├─────────────────────────────────────┤
│         Calico (CNI)                │  ← Pod Networking
├─────────────────────────────────────┤
│       Kubernetes (k3s)              │  ← Base
└─────────────────────────────────────┘
```