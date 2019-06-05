# Kubernetes on ContainerD on ARM64!

Still a WIP but feel free to try. Real quick and dirty at the moment.
When finished I will write a nice readme ;)

# Getting started:

## Create a hosts file looking like this:

```ini
[all]
node01 ansible_host=192.168.2.141
node02 ansible_host=192.168.2.142
node03 ansible_host=192.168.2.143
node04 ansible_host=192.168.2.144
node05 ansible_host=192.168.2.145
node06 ansible_host=192.168.2.145

[kubemaster]
node01
node02
node03

[kubenode]
node04
node05
node06
```

## Inspect and override some defaults in the defaults/main.yml
```ini
[all]
node01 ansible_host=192.168.2.141
node02 ansible_host=192.168.2.142
node03 ansible_host=192.168.2.143
node04 ansible_host=192.168.2.144
node05 ansible_host=192.168.2.145
node06 ansible_host=192.168.2.145

[all:vars]
kubernetes_version=1.12.7

[kubemaster]
node01
node02
node03

[kubenode]
node04
node05
node06
```

## Run the playbook and watch the magic happen

```
ansible-playbook -i hosts.ini playbook.yml
```

## Troubleshooting

### CoreDNS isn't starting

When you see this error on CoreDNS startup:
```
Events:
  Type     Reason                  Age                   From               Message
  ----     ------                  ----                  ----               -------
  Warning  FailedScheduling        29m (x3 over 29m)     default-scheduler  0/1 nodes are available: 1 node(s) had taints that the pod didn't tolerate.
  Normal   Scheduled               29m                   default-scheduler  Successfully assigned kube-system/coredns-fb8b8dccf-mfvrr to node01
  Warning  FailedCreatePodSandBox  29m                   kubelet, node01    Failed create pod sandbox: rpc error: code = Unknown desc = failed to setup network for sandbox "adab93f573607c9949f98c80ad8dd8d93bbbd8b2d489ae438d96469c0e6ba261": failed to find plugin "loopback" in path [/opt/cni/bin]
```

You need to reinstall the kubernetes-cni packages
```
apt-get install --reinstall kubernetes-cni
```
