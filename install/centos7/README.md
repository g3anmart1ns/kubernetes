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

cat >>/etc/modules-load.d/crio.conf<<EOF
overlay
br_netfilter
EOF

modprobe overlay
modprobe br_netfilter

cat >>/etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
EOF

sysctl --system

}

</pre>
