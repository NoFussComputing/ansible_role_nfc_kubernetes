---
title: Kubernetes
description: No Fuss Computings Ansible role nfc_kubernetes
date: 2023-10-24
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/roles/kubernetes
---

This Ansible role is designed to deploy a K3s Kubernetes cluster. Without adding cluster configuration this role will install K3s as a single node cluster. To deploy a multi-node cluster add your configuration, K3s will be installed on all nodes. On completion you will have fully configured cluster in a state ready to use. This role can be used with our [our playbooks](../../playbooks/index.md) or comes included, along with the playbook within our [Ansible Execution Environment](../../execution_environment/index.md).


## Role Details

| Item| Value | Description |
|:---|:---:|:---|
| Dependent Roles | _None_ | |
| Optional Roles | _nfc_firewall_ | Used to setup the firewall for kubernetes. |
| Idempotent | _Yes_ |  |
| Stats Available | _Not Yet_ |  |
| Tags | _Nil_ |  |
| Requirements | _Gather Facts_  |  |
|   | _become_ |  |


## Features

- CNI Setup, calico including `calicoctl` plugin

    > `kubectl calico ....` instead of `calicoctl ....`

- Configurable:

    - Container Registries

    - ectd deployment
    
        - etcd snapshot cron schedule

        - etcd snapshot retention
    
    - Cluster Domain

    - Configure System reserved CPU, Storage and Memory.

    - Node Labels

    - Node Taints

    - Service Load Balancer Namespace

- Encryption between nodes (Wireguard)

- [Firewall configured for kubernetes host](firewall.md)

- Multi-node Deployment

- OpenID Connect SSO Authentication

- [Basic RBAC `ClusterRoles` and Bindings](rbac.md)

- _[ToDo-#5](https://gitlab.com/nofusscomputing/projects/ansible/kubernetes/-/issues/5)_ Restore backup on fresh install of a cluster

- Installs OLM for operator subscriptions

- Install MetalLB

- Install KubeVirt including `virtctl` plugin

    > `kubectl virt ....` instead of `virtctl ....`

- Install the Helm Binary


## Role Workflow

For a more probable than not success this role first installs/configures prime master, other master(s) and worker nodes using the following simplified workflow:

1. Download both install script and k3s binary to ansible controller

1. copy install script and k3s binary to host

1. Create required config files needed for installation

1. _(kubernetes prime master only)_ Add install required config files

1. Install kubernetes

1. _(kubernetes prime master only)_ Wait for kubernetes to be ready. Playbook is paused until `true`

1. Configure Kubernetes

1. Install Kubevirt

If the playbook is setup as per [our recommendation](ansible.md) step 2 onwards is first done on master nodes then worker nodes.

!!! tip
    If you prefer to manually restart the kubernetes service the following variables can be set to prevent a restart of the kubernetes service

    ``` yaml
    nfc_kubernetes_no_restart: false
    nfc_kubernetes_no_restart_master: false
    nfc_kubernetes_no_restart_prime: false
    nfc_kubernetes_no_restart_slave: false
    ```
    _See default variables below for explanation of each variable if it's not evident enough._


## Default Variables


``` yaml title="defaults/main.yaml" linenums="1"

--8<-- "defaults/main.yml"

```