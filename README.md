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

```
ansible [core 2.12.0]
  config file = /home/pmavrikos/HackathonAnsible/ansible.cfg
  configured module search path = ['/home/pmavrikos/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/pmavrikos/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jan  8 2026, 06:52:19) [GCC 11.4.0]
  jinja version = 3.0.3
  libyaml = True
```