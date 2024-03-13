---
title: Ansible
description: No Fuss Computings Ansible role nfc_kubernetes Ansible docs
date: 2023-10-24
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/roles/kubernetes
---

This page intends to describe/explain the setup of ansible for this role. 

## Inventory Setup

There are many ways to layout your inventory within Ansible. To take full advantage of this role the following could be used:

- A aroup containing all master nodes

- A group containing all worker nodes

- a group containing all nodes for a cluster

- All groups above made a subordinate of a master group

- variable `ansible_host`. _can be either DNS name, IPv4/IPv6 Address_

    !!! info Info
        The nfc_kubernetes role uses this field for any configuration that requires a hostname. You are strongly encouraged to use DNS name and the DNS name be resolveable for each host accessing to the host in question. Using DNS host name is of paramount importance for a host with dynamic DHCP being used.

- variable `nfc_role_kubernetes_master` _boolean_ set for all host that are master nodes.

- hosts that require Kubernetes API access added to variable `kubernetes_config.cluster.access`

An example inventory file that would suffice.

``` yaml
all:
  hosts:
    localhost:
      vars:
        ansible_connection: local
  children:

    kubernetes:
      children:

        k3s:
          hosts:


        k8s:
          hosts:


        kubernetes_cluster:
          children:

            kubernetes_cluster_{cluster_name_here}:
              hosts:


        kubernetes_master:
          hosts:


        kubernetes_worker:
          hosts:

```

The reasoning for the layout above is:

- group `kubernetes` used as a selector within playbook or limitor when running a playbook to cover all kubernetes hosts.

- groups `kubernetes`, `k3s`, `k8s` and `kubernetes_cluster_{cluster_name_here}` used for variable files (`inventory/group_vars/{group_name}.yaml`). with the latter containing all settings for the cluster in question.

- Hosts are added to ALL groups relevent to them.


The following group variable files will also need to be created:

- `inventory/group_vars/all.yaml` Variables applicable to all hosts

- `inventory/group_vars/kubernetes.yaml` software versions for kubernetes 

- `inventory/group_vars/kubernetes_cluster_{cluster_name_here}.yaml` cluster configuration


## Playbooks Setup

Whilst there are many ways to skin a cat, using the inventory layout as defined above, with the creation of playbooks as detailed below is a possible solution covering most basis' of using this role.

playbooks/kubernetes.yaml

``` yaml
---
- name: Kubernetes Group and sub-groups
  hosts: "{{ groups.kubernetes }}"
  gather_facts: true

  roles: []

  - name: Kubernetes Master
  import_playbook: kubernetes/master.yaml

- name: Kubernetes Worker
  import_playbook: kubernetes/worker.yaml
```

playbooks/kubernetes/master.yaml
``` yaml
---
- name: Kubernetes Master Nodes
  hosts: "{{ kubernetes_master }}"
  gather_facts: true

  roles: 
    - name: Kubernetes Setup
      role: nfc_kubernetes

```

playbooks/kubernetes/worker.yaml
``` yaml
---
- name: Kubernetes worker Nodes
  hosts: "{{ kubernetes_worker }}"
  gather_facts: true

  roles:
    - name: Kubernetes Setup
      role: nfc_kubernetes

```

Running the above playbooks with the inventory setup allows the following and more:

- Setup Kubernetes on all applicable kubernetes hosts

    > `ansible-playbook -i inventory/production  playbooks/kubernetes.yaml`

- Setup kubernetes cluster `{cluster_name}`

    > `ansible-playbook --limit kubernetes_cluster_{cluster_name_here} -i inventory/production  playbooks/kubernetes.yaml`

- Setup all Kubernetes master nodes, regardless of cluster

    > `ansible-playbook --limit kubernetes_master -i inventory/production  playbooks/kubernetes.yaml`
