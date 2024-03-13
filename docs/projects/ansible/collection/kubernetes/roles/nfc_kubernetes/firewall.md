---
title: Firewall
description: No Fuss Computings Ansible role nfc_kubernetes
date: 2023-10-24
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/collections/kubernetes
---

This role include logic to generate firewall rules for iptables. Both IPv4 and IPv6 rules are generated. to survive reboots or network cable disconects, a script is created and added to the `if-up.d.` This enables that each time the interface is brought up, the firewall rules are applied. For a list of the firewall rules applied see the [K3s documentation](https://docs.k3s.io/installation/requirements#inbound-rules-for-k3s-server-nodes)

Rules generation workflow:

- itertes over all kubernetes hosts

- adds rules if host is masters for worker access

- adds rules if worker for all node access

- adds rules for additional hosts to access kubernetes api

What you end up with:

- chains for each area of access to the cluster

- The input table contains the jump to each chain, based off of destination port and protocol

- each chain returns to INPUT table for further processing.

!!! danger Security
    The way the rules are created and applied, they all return to the `INPUT` table for further processing. If the `INPUT` tables default policy is `ACCEPT`. Regardless of the firewall rules in place, Any host with network access to the kubernetes host can access the desired service without needing a rule to grant access.

    **Recommendation:** Set the `INPUT` tables default policy to `DROP`

!!! info Info
    If a DNS name is used for any off the address' a DNS lookup is done for both IPv4 and IPv6 adding the first host found to the applicable chain.


## Providing access to the cluster

No special skill is required apart from adding the host to grant access to the right list variable. i.e.

``` yaml
kubernetes_config:
  cluster:
    access:
      - '192.168.1.1'
      - 'my.hostname.com'
```

Any host that is added to the `access` list will be granted access to the Kubernetes API. Hosts in this list are intended to be the hosts your end users are on. If you join a new node to the cluster, the applicable firewall rules will automagically generated and added to each hosts firewall. It's important that when adding anew node to the cluster, that the playbook is run agains all nodes of the cluster, not just the new node. Failing to do so, will have the existing nodes block access to the new node due to missing firewall rules.


!!! tip Tip
    When manually adding a host use insert `-I` not append `-A` as the last rule must be `-j RETURN`

    exmple: `-I {chain_name} -s {hostname/ipaddress} -j ACCEPT`



Protocol	Port	Source	Destination	Description
TCP	2379-2380	Servers	Servers	Required only for HA with embedded etcd
TCP	6443	Agents	Servers	K3s supervisor and Kubernetes API Server
UDP	8472	All nodes	All nodes	Required only for Flannel VXLAN
TCP	10250	All nodes	All nodes	Kubelet metrics
UDP	51820	All nodes	All nodes	Required only for Flannel Wireguard with IPv4
UDP	51821	All nodes	All nodes	Required only for Flannel Wireguard with IPv6






