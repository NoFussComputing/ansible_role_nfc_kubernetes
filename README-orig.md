# Kubernetes Ansible Playbook


## Additional changes

- `SystemdCgroup = false` -> `SystemdCgroup = true` [See this comment](https://github.com/kubernetes/kubernetes/issues/110177#issuecomment-1161647736)

## Tags

This role has been setup to take advantage of Ansible tags. The use of these tags enables finer control over what tasks are run. By design, when you set a task, only what is required for the tag is run.

available tags are as follows

- `containerregistry` apply container/docker registry settings
- `firewall` apply firewall settings (firewall name/type independent)
- `install` Run every task within the role. this is the same as omitting `--tags`
- `iptables` apply iptables settings
- `manifest` Apply/remove kubernetes manifests
- `namespace` Apply/remove kubernetes namespaces
- `nodelabels` Apply/remove kubernetes node labels
- `taints` Apply/remove kubernetes taints

!!! tip
    if you intend on running the `install` tag, you can omit the `--tags` flag from the ansible tag all togther

!!! alert
    the first time this playbook is run if cli switch `--extra-vars "init=true"` is used with `init` either a bool true/false, the manifests will not be applied. this is to enable the kubernetes to be fully setup prior to applying manifests that may prevent successful completion of the play.

## command Cheatsheet

- `crictl --runtime-endpoint unix:///run/containerd/containerd.sock images` list all container images on the host

## Links / References

- ContainerD Configuration

  - [Registry Configuration](https://github.com/containerd/containerd/blob/7cd72cce99c8d3b938c1b763c2744a0b699028ab/docs/cri/config.md#registry-configuration)

  - [Configuring ContainerD registries](https://github.com/containerd/containerd/blob/7cd72cce99c8d3b938c1b763c2744a0b699028ab/docs/hosts.md#cri)
