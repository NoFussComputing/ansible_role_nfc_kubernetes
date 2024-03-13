---
title: RBAC
description: No Fuss Computings Ansible role nfc_kubernetes RBAC documentation.
date: 2023-10-29
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/collections/kubernetes
---

As part of this roles workflow, A set of Clester Roles and Cluster Bindings are deployed and ready to use. The intent of these roles is to create a default set of roles that only require the authorization system to provide the users groups. As they have been defined as Cluster Roles you can bind to both cluster and/or namespace.  
A minimum access required princible has been adopted in the creation of these roles. With the roles designed to be for whom would access/use the cluster (An End user).

!!! tip
    All Deployed `ClusterRole` include a labels `authorization/description` and `authorization/target` explaining their intended purpose and where they a recommended for binding.


Currently the following roles are deployed as part of this Anible role:

- authorization:namespace:read

    > Full read access to all objects except secrets

- authorization:full

    > Full read/write access to all objects including secrets

- authorization:namespace:owner

    > Full read/write access to all objects including secrets

- authorization:cluster:view-metrics

    > View node and pod metrics

- **[ToDo-#6](https://gitlab.com/nofusscomputing/projects/ansible/kubernetes/-/issues/6)** authorization:cluster:admin

    > Configure the cluster with this not including anything that can be deployed.

