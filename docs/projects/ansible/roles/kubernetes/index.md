---
title: Kubernetes
description: No Fuss Computings Ansible role nfc_kubernetes
date: 2023-10-24
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/roles/kubernetes
---

This Ansible roles purpose is to install and configure Kubernetes with configuration from code. You can also use [our playbooks](../../playbooks/index.md) to deploy using this role. this is especially useful if you are also using [our Ansible Execution Environment](../../execution_environment/index.md) 


## Features

This role deploys a K3s cluster. In addition it has the following features:

- CNI Setup

- Configurable:

    - Container Registries

    - etcd snapshot cron schedule

    - etcd snapshot retention
    
    - Cluster Domain

    - Configure System reserved CPU, Storage and Memory.

    - Node Labels

    - Node Taints

    - Service Load Balancer Namespace

- _[ToDo-#3](https://gitlab.com/nofusscomputing/projects/ansible/kubernetes/-/issues/3)_ Encryption between nodes (Wireguard)

- [Firewall configured for kubernetes host](firewall.md)

- _[ToDo-#2](https://gitlab.com/nofusscomputing/projects/ansible/kubernetes/-/issues/2)_ Multi-node Deployment

- OpenID Connect SSO Authentication

- [Basic RBAC `ClusterRoles` and Bindings](rbac.md)

- _[ToDo-#5](https://gitlab.com/nofusscomputing/projects/ansible/kubernetes/-/issues/5)_ Restore backup on fresh install of a cluster

- Installs OLM for operator subscriptions


## Role Workflow

For a more probable than not success this role first installs/configures prime master, other master(s) and worker nodes using the following simplified workflow:

1. Download both install script and k3s binary to ansible controller

1. copy install script and k3s binary to host

1. Create required config files needed for installation

1. _(kubernetes prime master only)_ Add install required config files

1. Install kubernetes

1. _(kubernetes prime master only)_ Wait for kubernetes to be ready. Playbook is paused until `true`

1. Configure Kubernetes

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