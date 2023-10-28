---
title: Kubernetes Ansible Role
description: No Fuss Computings Ansible role nfc_kubernetes
date: 2023-10-24
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/roles/kubernetes
---



Expected inventory setup:

- each host has a host_vars file with `ansible_host` defined. _can be either DNS name, IPv4/IPv6 Address_
- `k3s` host group with all hosts part of this group
- `kubernetes_master` host group with all master nodes part of

- variable `Kubernetes_Master` _boolean_ set for all host that are master nodes.

- hosts that require Kubernetes API access added to variable `kubernetes_config.cluster.access`
