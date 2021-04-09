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
