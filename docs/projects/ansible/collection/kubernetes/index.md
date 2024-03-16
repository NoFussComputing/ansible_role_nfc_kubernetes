---
title: Kubernetes
description: No Fuss Computings Ansible Collection Kubernetes
date: 2024-03-13
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/collections/kubernetes
---

<span style="text-align: center;">

![Project Status - Active](https://img.shields.io/badge/Project%20Status-Active-green?logo=gitlab&style=plastic)


![Gitlab build status - stable](https://img.shields.io/badge/dynamic/json?color=ff782e&label=Build&query=0.status&url=https%3A%2F%2Fgitlab.com%2Fapi%2Fv4%2Fprojects%2F51640029%2Fpipelines%3Fref%3Dmaster&logo=gitlab&style=plastic) ![Gitlab build status - development](https://img.shields.io/badge/dynamic/json?color=ff782e&label=Build&query=0.status&url=https%3A%2F%2Fgitlab.com%2Fapi%2Fv4%2Fprojects%2F51640029%2Fpipelines%3Fref%3Ddevelopment&logo=gitlab&style=plastic)


[![Downloads](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv3%2Fplugin%2Fansible%2Fcontent%2Fpublished%2Fcollections%2Findex%2Fnofusscomputing%2Fkubernetes%2F&query=%24.download_count&style=plastic&logo=ansible&logoColor=white&label=Galaxy%20Downloads&labelColor=black&color=cyan)](https://galaxy.ansible.com/ui/repo/published/nofusscomputing/kubernetes/)


</span>

This Ansible Collection is for installing a K3s Kubernetes cluster, both single and multi-node cluster deployments are supported. In addition to installing and configuring the firewall for the node. for further information on the firewall config please see the [firewall docs](../firewall/index.md)


## Installation

To install this collection use `ansible-galaxy collection install nofusscomputing.kubernetes`


## Features

Most of the features of this collection are from the included role `nfc_kubernetes`, please [view its page for feature details](roles/nfc_kubernetes/index.md).


## Using this collection

This collection has been designed to be a complete and self-contained management tool for a K3s kubernetes cluster.

## K3s Kubernetes Installation

By default the install playbook will install to localhost.

``` bash

ansible-playbook nofusscomputing.kubernetes.install

```

The install playbook has a dynamic `hosts` key. This has been done to specifically support running the playbook from AWX and being able to populate the field from the survey feature. Order of precedence for the host variable is as follows:

- `nfc_pb_host` set to any valid value that a playbook `hosts` key can accept

- `nfc_pb_kubernetes_cluster_name` with the name of the cluster. This variable is appended to string `kubernetes_cluster_` to serve as a group name for the cluster to be installed. i.e. for a cluster called `prime`, the group name would be set to `kubernetes_cluster_prime`

- `--limit` specified at runtime

- `localhost`

For the available variables please view the [nfc_kubernetes role docs](roles/nfc_kubernetes/index.md#default-variables)



