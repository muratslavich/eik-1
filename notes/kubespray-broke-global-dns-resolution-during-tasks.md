# kubespray broke global dns resolution during tasks

{% embed url="https://github.com/kubernetes-sigs/kubespray/issues/2831" %}

{% embed url="https://github.com/kubernetes-sigs/kubespray/issues/3293" %}

```ansible
/inventory/my/grou_vars/all/all.yml

unsafe_show_log: True
upstream_dns_servers:
 - 8.8.8.8
 - 8.8.4.4
```

