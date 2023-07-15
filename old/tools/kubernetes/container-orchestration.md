# Container Orchestration

##

Application requirements:

* Fault tolerance
* On-demand scalability
* Optimal resource usage
* Auto discovery to automatically communicate with each other.
* Accessibility from the outside world
* Update and rollback without downtime

Today container orchestration tools:

* Amazon ECS - elastic container services
* Azure container instances
* Azure service fabric
* Kubernetes
* Marathon - Apache Mesos
* Nomad - Hashi corp
* Docker Swarm

Why we are using container orchestration

* we can manually maintain a couple of containers
* we can write scripts to manage a doze
* orchestrators make things much easier when it hundred or thousand containers

Orchestrators can

* grouping hosts
* schedule containers to run on hosts on resources availability
* enable containers to communicate with each other in the cluster
* bind containers and storage resources
* load balancing
* manage resource usage
* security inside containers
