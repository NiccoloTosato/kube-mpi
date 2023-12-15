# Vagrant setup

Pre-flight check:

```
$ sudo virsh net-define kub-devel-network.xml
$ sudo virsh net-start --network kub-devel
$ vagrant up
```

Note: `vagrant up` must be executed in the same folder of `Vagrantfile`.
