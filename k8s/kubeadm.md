### 概念扫盲

- [cgroups](https://tech.meituan.com/2015/03/31/cgroups.html) 

Linux内核提供的一种可以限制单个进程或者多个进程所使用资源的机制，可以对 cpu，内存等资源实现精细化的控制



### 生产环境

[Production environment](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#)

检查docker的cgroups配置，修改daemon.json

```bash
# Setup daemon.
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

mkdir -p /etc/systemd/system/docker.service.d

# Restart docker.
systemctl daemon-reload
systemctl restart docker
```



Somethings important you should check

- Unique hostname, MAC address, and product_uuid for every node. See [here](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#verify-the-mac-address-and-product-uuid-are-unique-for-every-node) for more details.
- Certain ports are open on your machines. See [here](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#check-required-ports) for more details.
- Swap disabled. You **MUST** disable swap in order for the kubelet to work properly.
  - [why disable swap](https://serverfault.com/questions/881517/why-disable-swap-on-kubernetes)
  - [https://github.com/kubernetes/kubernetes/issues/53533](



创建proxy

默认dashboard 采用clusterIP只能集群内部访问

```bash
nohub kubectl proxy  --port=[需要暴露的端口号] --address='[服务器IP]' --accept-hosts='^[外部访问服务器的IP]$'  >/dev/null 2>&1& 

nohup kubectl proxy --port=30000 --address='10.122.100.166' --accept-hosts='^*$' >/dev/null 2>&1&
```

