# VM setup

Provision kube nodes:

```
$ sudo virsh net-define kub-devel-network.xml
$ sudo virsh net-start --network kub-devel
$ vagrant up
```

Note: `vagrant up` must be executed in the same folder of `Vagrantfile`.

Pre-flight check:

```
# Load modules
modprobe overlay
modprobe br_netfilter

# make load permanent
cat <<EOF | tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

# change kernel parameters
cat <<EOF |  tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

# Load kernel parameters at runtime
sysctl --system

# disable zram
touch /etc/systemd/zram-generator.conf
swapoff -a

# Security? What security? Disable SElinux.
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```

*Totally inspired to [this repo]( https://github.com/Foundations-of-HPC/Cloud-advanced-2023/blob/main/live-demos/kube-installation/notes.org)* 


