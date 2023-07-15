# Kubernetes

![](https://habrastorage.org/getpro/habr/post\_images/735/b88/a2a/735b88a2a717e9c01bfc197f3c1b20fd.png)\* API Server. Выполнение обращений к этому серверу — единственный способ взаимодействия с кластером

* Kubelet. Это — агент, который осуществляет мониторинг контейнеров, и взаимодействует с главным узлом
* Pod - минимальная развёртываемая вычислительная единица, может содержать несколько контейнеров, которые используют одну среду выполнения.

![](https://habrastorage.org/getpro/habr/post\_images/4f6/2a4/bb1/4f62a4bb18bddccc49a9d224a4aa919d.png)1. Each pod have in cluster have unique IP address 2. In the pod containers are interacting through 3. Containers in pods share data storage volumes, IP address, port numbers, IPC namespace.

```
## pod.yaml

apiVersion: v1
kind: Pod                                            # 1
metadata:
  name: sa-frontend                                  # 2
spec:                                                # 3
  containers:
    - image: rinormaloku/sentiment-analysis-frontend # 4
      name: sa-frontend                              # 5
      ports:
        - containerPort: 80
```

1. kind - define type of Kubernetes resource (pod)
2. name - resource name
3. spec - definition of resource state
4. image - container image which run in the pod
5. name - name for container in pod
6. containerPort - port for container listening

Creating pod

```
$ kubectl create -f sa-frontend-pod.yaml
```

Forward port

```
$ kubectl port-forward sa-frontend 88:80
```

## **Services**

![](https://habrastorage.org/getpro/habr/post\_images/bbd/95f/bd8/bbd95fbd8562bed4a09ab4930a20f98d.png)

* labels - for service targets

![](https://habrastorage.org/getpro/habr/post\_images/bb9/fcf/f0c/bb9fcff0cded591f1a5ab8a0b825245a.png)

After modification cofig file you can apply changes

```
$ kubectl apply -f sa-frontend-pod.yaml
```

Create service - load balancing

```
## service.yaml

apiVersion: v1
kind: Service              # 1
metadata:
  name: sa-frontend-lb
spec:
  type: LoadBalancer       # 2
  ports:
  - port: 80               # 3
    protocol: TCP          # 4
    targetPort: 80         # 5
  selector:                # 6
    app: sa-frontend       # 7
```

1. kind - service
2. type - LoadBalancer
3. Port where service accepts requests
4. Service protocol
5. Port to which coming requests are redirecting
6. The Selector define with witch pods service should working
7. Define app label

## Deployment

```
apiVersion: extensions/v1beta1
kind: Deployment                                          # 1
metadata:
  name: sa-frontend
spec:
  replicas: 2                                             # 2
  minReadySeconds: 15
  strategy:
    type: RollingUpdate                                   # 3
    rollingUpdate: 
      maxUnavailable: 1                                   # 4
      maxSurge: 1                                         # 5
  template:                                               # 6
    metadata:
      labels:
        app: sa-frontend                                  # 7
    spec:
      containers:
        - image: rinormaloku/sentiment-analysis-frontend
          imagePullPolicy: Always                         # 8
          name: sa-frontend
          ports:
            - containerPort: 80
          env:
            - name: SA_LOGIC_API_URL
              value: "http://sa-logic"
```

1. kind - deployment
2. replicas - instances count
3. type - new version migration strategy, RollingUpdate provides zero system downtime
4. maxUnavailable - max unavailable pods during system update
5. maxSurge - max pods counts that can be added after update
6. template - pod template that will be used for creating new pods
7. label for creating pods
8. imagePullPolicy - always pull image from repository

```
$ kubectl apply -f sa-frontend-deployment.yaml

$ kubectl rollout history deployment sa-frontend
$ kubectl rollout undo deployment sa-frontend --to-revision=1
```

```
$ kubectl delete pod <pod-name>
```
