---
title: Release Notes
description: No Fuss Computings Ansible role nfc_kubernetes
date: 2024-01-31
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/roles/kubernetes
---

This document details any changes that have occured that may impact users of this role. It's a rolling document and will be amended from time to time.


## Changes with an impact

- _**13 Mar 2024**_ Container Images now a dictionary. This role has two images `kubevirt_operator` and `tigera_operator`.

    - All Images are stored in dictionary `nfc_role_kubernetes_container_images` with each image using its own dictionary with mandatory keys `registry`, `image` and `tag`. This change has been made to cater for those whom store their images within their inventory as a dict of dict. For instance to use your inventory image declare variable `nfc_role_kubernetes_container_images.kubevirt_operator: my_images.my_kubevirt_dict` as an example.

    - A lot of variables have been updated. To view what has changed, please see `defaults/main.yaml` in [MR !35](https://gitlab.com/nofusscomputing/projects/ansible/kubernetes/-/merge_requests/35)

- _**31 Jan 2024**_ Calico CNI deployment has been migrated to use the calico operator. 

    - All new cluster installations will be deployed with the operator

        - Existing deployments will be required to run a deployment with job tag `operator_migrate_calico` to migrate their deployment to the operator
        
        - if an issue occurs with the migration it can be rolled back by `kubectl delete -f` for all manifests in the `/var/lib/rancher/k3s/ansible` directory and redeploying with job tag `calico_manifest`. This re-deploys calico using the current manifest.
        
        - This tag will be removed in the future at no set date.

    - `ServiceLB` / `klipperLB` no longer deploys by default and to deploy it variable `nfc_kubernetes_enable_servicelb` must be set `true`
