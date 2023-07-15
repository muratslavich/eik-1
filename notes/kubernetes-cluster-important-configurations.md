# Kubernetes Cluster Important Configurations

### &#x20;<a href="#kubernetes-cluster-important-configurations" id="kubernetes-cluster-important-configurations"></a>

Following are the important cluster configurations you should know.

| Configuration                                                                    | Location                     |
| -------------------------------------------------------------------------------- | ---------------------------- |
| Static Pods Location (etcd, api-server, controller manager and scheduler)        | /etc/kubernetes/manifests    |
| TLS Certificates location (kubernetes-ca, etcd-ca and kubernetes-front-proxy-ca) | /etc/kubernetes/pki          |
| Admin Kubeconfig File                                                            | /etc/kubernetes/admin.conf   |
| Kubelet configuration                                                            | /var/lib/kubelet/config.yaml |
