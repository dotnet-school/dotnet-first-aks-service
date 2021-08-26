# Kubernetes

**How K8 works ?**

- It uses declarative style,
- i.e. **we tell what is the desired state for a cluster using YAML declaration**
- and run the `kubectl apply`
- Kubernetes automactically makes changes to the cluster based on our desired state.



| What we need in our cluster ?                                | What Kubernetes resource we need ?               |
| ------------------------------------------------------------ | ------------------------------------------------ |
| Running app as a docker container                            | **Deployment** resource for app's docker image   |
| Running multiple instances of app and restarting them if they fail | **ReplicaSet**  (pary of deployment description) |
| Load balancer to receive traffic from internet (outside the cluster) | **Service** of type load balancer                |



### Kubernetes API versions

- When talking to Kubernetes, we alwasy define what API language are we talking in
- Managed Kubernetes like Azure Kubernetes will usually not have the latest API's
- When referring to docs, make sure you are looking at correct api versions



- **[Pods](./Pods.md)**

  > - *How are Pods created ?*
  > - *How are pods destroyed ?*
  > - *Interacting with Pods using CLI*
  > - *Liveness probe*

- **[Replica Sets](./ReplicaSets.md)**

  > - *Self healing sets of Pods*
  > - *Cascade deletion of resources*
  > - *Deployment > ReplicaSets > Pod (creations)*

- **[Services](./Services.md)**

  > - Why are services required ?
  > - How are serivices created ?
  > - Endpoint Controller, kubeproxy, kube-dns addon, proxy tables, skydns

- **[Deployments](./Deployments)**

  > - Why we need deployments ?
  > - Zero downtime deployments
  > - See rollout history
  > - Rolling back a faulty release