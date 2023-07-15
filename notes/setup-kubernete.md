# Setup Kubernete

{% embed url="https://devopscube.com/setup-kubernetes-cluster-kubeadm/" %}

* После установки
  * сертификаты [https://app.gitbook.com/s/lUJ4TnM0BYx3EqJIt864/make-metrics-server-work-out-of-the-box-with-kubeadm/](make-metrics-server-work-out-of-the-box-with-kubeadm.md)
  * установил [https://github.com/prometheus-operator/kube-prometheus/](https://github.com/prometheus-operator/kube-prometheus/)
  * nginx-ingress [https://github.com/kubernetes/ingress-nginx](https://github.com/kubernetes/ingress-nginx) NodePort
  * проблема открытия порта 80 для кубера [https://github.com/kubernetes/ingress-nginx/blob/main/docs/deploy/baremetal.md](https://github.com/kubernetes/ingress-nginx/blob/main/docs/deploy/baremetal.md)



<details>

<summary>plain nginx proxy</summary>

```


# Required for Jenkins websocket agents
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

server {
    listen 80;
    server_name portainer.protonmath.ru;

    location / {
        proxy_pass http://94.250.249.56:30394;
        proxy_set_header Host $host;  
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header   Upgrade            $http_upgrade;
        proxy_set_header   Connection         $connection_upgrade;
    }
}

```

</details>

