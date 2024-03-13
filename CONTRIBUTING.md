# Contribution Guide



## Updating components with a remote source

Some components within this role are sourced from a remote source. To update them to the latest release use the following commands.

> Ensure that before committing the update remote files to the repository, that no features have been removed that were added.


### Kubevirt

``` bash

export KUBEVIRT_RELEASE='<kubevirt release i.e. v1.2.0>'

# From within the templates directory
wget https://github.com/kubevirt/kubevirt/releases/download/${KUBEVIRT_RELEASE}/kubevirt-operator.yaml -O kubevirt-operator.yaml.j2

# From within the templates directory
wget https://github.com/kubevirt/kubevirt/releases/download/${KUBEVIRT_RELEASE}/kubevirt-cr.yaml -O kubevirt-cr.yaml.j2

```
