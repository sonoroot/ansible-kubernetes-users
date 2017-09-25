# User Deployment (Kubernetes cluster 1.7+)

- Requirements: a running Kubernetes cluster (1.7+)

    This ansible role uses openssl to handle certificates used for Kubernetes authentication.

    CSRs are approved by the internal CA of Kubernetes.

    All openssl commands and certifications handling are performed on the managing node.

How to add a new Kubernetes user:
---------------------------------
- edit `~/../kubernetes_rbac_users/vars/env.yaml` and fill your master IP address or DNS name
- edit `~/../kubernetes_rbac_users/vars/users.yml` (comment out exsiting users, you might want to keep the group)

```yaml
---
user: kirk
group: ncc1701
# user: "spock"
# user: "mccoy"
# user: "bones"
# user: "uhura"
```

- run the playbook:
```
ansible-playbook -i inventory create_users.yml
```

When the playbook run is finished, the file `~/k8s-users/<user>/kubeconfig` will be deployed; 

- to access the kubernetes cluster as `<user>`:

```bash
export KUBECONFIG=~/k8s-users/<user>/kubeconfig
``` 

```bash
kubectl get pods
```

NOTE:
Remember to define RBAC for the new users, as newly created users they are denied everything.

References:

https://kubernetes.io/docs/admin/authentication/

https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/