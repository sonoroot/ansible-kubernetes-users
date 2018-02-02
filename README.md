# User Deployment (Kubernetes cluster 1.7+)

## Requirements
- A running Kubernetes cluster (1.7+)

    This ansible role uses openssl to handle certificates used for Kubernetes authentication.

    CSRs are approved by the internal CA of Kubernetes.

    All openssl commands and certifications handling are performed on the managing node.

## How to add a new Kubernetes user:
---------------------------------
- edit `roles/kubernetes_rbac_users/vars/env.yaml` and specify your master IP address or DNS name
- edit `roles/kubernetes_rbac_users/vars/users.yml` (comment out already deployed users, you might want to keep the group)

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

When the playbook run is finished, the file `~/k8s-users/<user>/kubeconfig` will be deployed locally on your management node; 

- to access the kubernetes cluster as `<user>`:

```bash
export KUBECONFIG=~/k8s-users/<user>/kubeconfig
``` 

```bash
kubectl get pods
```

NOTE:
Remember to define RBAC for the new users, as newly created users they are denied everything.
For example to give a user admin rights within a specific namespace:

```bash
# become admin user
$ export KUBECONFIG=/etc/kubernetes/admin.conf
#TODO: add example of giving admin rights within a specific namespace
```

References:

https://kubernetes.io/docs/admin/authentication/

https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/
