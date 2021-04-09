# Install Kubernetes - CentOS 7

|Role|FQDN|IP|OS|RAM|CPU|
|------|-------------------------|----------|------|-|-|
|Master|kube-ctrl-pl-00.example.com|192.0.2.10|CentOS|4|2|
|Worker|kube-worker-00.example.com|192.0.2.20|CentOS|4|2|
|Worker|kube-worker-01.example.com|192.0.2.21|CentOS|4|2|

<pre>
yum update -y
</pre>
<pre>
yum install -y vim wget curl telnet rsync screen yum-utils lvm2 device-mapper-persistent-data nfs-utils bash-completion
</pre>

<pre>
{

cat >> /etc/hosts << EOF
192.0.2.10    kube-ctrl-pl-00.juntotelecom.com.br     kube-ctrl-pl-00
192.0.2.20    kube-worker-00.juntotelecom.com.br      kube-worker-00
192.0.2.21	  kube-worker-02.juntotelecom.com.br      kube-worker-02
EOF

}
</pre>

<pre>
{

pvcreate /dev/sdb
vgcreate containersvg /dev/sdb
lvcreate -l 100%FREE -n containerslv containersvg
mkfs.xfs /dev/mapper/containersvg-containerslv
mkdir /var/lib/containers
echo "/dev/mapper/containersvg-containerslv /var/lib/containers xfs defaults 0 0" >> /etc/fstab
mount -a
df -h | grep containers

}
</pre>

<pre>
systemctl disable firewalld --now
</pre>

<pre>

{

cat >> /etc/modules-load.d/crio.conf << EOF
overlay
br_netfilter
EOF

modprobe overlay
modprobe br_netfilter

}
</pre>

<pre>
{

cat >> /etc/sysctl.d/kubernetes.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
EOF

sysctl --system

}

</pre>

<pre>
lsmod | grep br_netfilter
lsmod | grep overlay
</pre>

<pre>

{

cat << EOF | tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF

}
</pre>

<pre>
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
</pre>

<pre>
swapoff -a
cp -f /etc/fstab /etc/fstab.bak
sed -i '/swap/d' /etc/fstab
</pre>

<pre>
yum repolist -y
</pre>

<pre>
OS=CentOS_7
VERSION=1.20
</pre>
