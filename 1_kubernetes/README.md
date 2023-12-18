# k8s deployment

### Login

Login into `k01` VM:

```
$ vagrant ssh k01
$ sudo su
```

### Requirements

Add the required repo and installation:
```
# attention to exclude!!
root@k01$ cat << EOF | tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.28/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.28/rpm/repodata/repomd.xml.key
exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni
EOF
# utils 
dnf install iproute-tc wget -y
# CRI-o
dnf install crio -y
# real kube
dnf install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
```

## Installation

Enable services and bootstrap the first node of the cluster:

```
sed -i 's/10.85.0.0\/16/10.17.0.0\/16/' /etc/cni/net.d/100-crio-bridge.conflist
systemctl enable --now crio
systemctl enable --now kubelet

kubeadm init --pod-network-cidr=10.17.0.0/16
# --services-cidr=10.96.0.0/12 /default
# --control-plane-endpoint 192.168.132.80 /needed for HA
```

Configure kubectl in order to use it from the host:

*Optionally:"* you can enable kubectl and consequently k9s to work from you host machine: 

```
scp -i ./ssh/id_rsa root@192.168.133.80:/etc/kubernetes/admin.conf $HOME/.kube/config
```

Finally get the command to join the second node into the cluster:

```
kubeadm token create --print-join-command
```


## Join the second node:

Use the command obtained above to join the second node into the cluster:

```
kubeadm join 192.168.121.182:6443 --token nqezgs.ryjdmutyb22xud5b --discovery-token-ca-cert-hash sha256:f171f1937e5c4fce9b79a1996dfbf70063632489668a79c1ad7fe91c47c61a78
```



*Totally inspired to [this repo]( https://github.com/Foundations-of-HPC/Cloud-advanced-2023/blob/main/live-demos/kube-installation/notes.org)* 


