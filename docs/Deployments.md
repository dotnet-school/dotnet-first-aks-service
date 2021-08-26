# Deployments

- Make sure we can release with zero downtime
- Pods/Replicasets should not be created directly, but only using deployments
- `kubectl apply` should be used for applications that do not update frequently
- `kubectl set image` is better for CI/CD

![image-20201103214629141](/Users/dawn/projects/project_2020/kubernetes/images/deployments-overview.png)



### Sample Deployment YAML

```yaml
# Create pods and replicasets and rollback as required
apiVersion : apps/v1
kind: Deployment
metadata:
  name: oapiserviceone 
spec:
  selector:
    matchLabels:
      app: oapiserviceone
  minReadySeconds: 1          # wait for 1 sec before liveness probe
  progressDeadlineSeconds: 60 # ??
  revisionHistoryLimit: 5     # ??
  replicas: 2                 # Replica set
  strategy:
    type: RollingUpdate       # Zero downtime deployment
    rollingUpdate:
      maxSurge: 1             # maxiumum 3 pods at any time
      maxUnavailable: 1       # minimum 1 pod at any time
  template:
    metadata:
      labels:
        app: oapiserviceone 
    spec:
      containers:
        - name: oapiserviceone 
          image: saxolab.azurecr.io/oapiserviceone # Image name for pods
          ports:
          - containerPort: 80
```



### What happens when a new deployment is run 

- kubectl sends request to API server to create deployment from yaml
- Deployment Controller detects new deployment definition is created
- Deployment Controller creates a replica set for the deployment
- Replication controller detects new replica definition 
- Replicatoin Controller creates unassigned pods
- Scheduler detects new pods and assigns them to nodes
- kubelet on nodes detect pods being assigned to them
- kubelet downloads images and runs pods

![image-20201103215340296](/Users/dawn/projects/project_2020/kubernetes/images/deployment-creation.png)





- **minReadySeconds**

  0 by default. Kubernetes waits for this many seconds before starting a liveness probe on the pods

  

- **revisionHistoryLimit**

  10 by default. Number of old replicasets that we can rollback to. 

  

- **progressDeadlineSeconds**

  If the number of pods do not come up in this many seconds, Kubernetes marks the deployment as stuck.



- **strategy.type**

  `RollingUpdate` or `Recreate`. Default is `RollingUpdate`. 

  

- **rollingUpdate.maxSurge**

  Can be `%` or count. Default is `25%`. Maxiumum number of  pods at any point in time

  e.g. if `replica: 100` and `maxSurge: 10%`, then number of pods never exceed 110 at any point in time.

  

- **rollingUpdate.maxUnavailable**

  Can be `%` or count. Default is `25%`. Minimum number of  pods at any point in time. 

  e.g. if `replica: 100` and `maxUnavailable: 10%`, then number of pods never go less than 90 at any point in time.



### Gradual creationg of replica sets

After running a deployment, do a describe. The last lines of events will look like 

![image-20201103233646779](/Users/dawn/projects/project_2020/kubernetes/images/deployment-evets.png)

It gradually decreases the size or replicaset of last deployment, and keeps increasing pods in current. Till current is at replica count and previous is 0.



### View rollout history (revisions)

```bash
kubectl rollout history -f deploy/go-demo-2-api.yml

# deployments "go-demo-2-api"
# REVISION CHANGE-CAUSE
# 1        kubectl create --filename=deploy/go-demo-2-api.yml --record=true
# 2        kubectl set image api=vfarcic/go-demo-2:2.0 --filename=deploy/go-demo-2-api.yml
```

It shows us what was released, using what commands.

It only works, if the deployment was created using flag `--record=true`



### Rolling back a release

```bash
# Undo last deployment
kubectl rollout undo -f deploy/go-demo-2-api.yml

# Restore a previous deployment
kubectl rollout undo \
    -f deploy/go-demo-2-api.yml \
    --to-revision=2
```



### Update multiple delpoyments at once

we can use the label to update mulitple deployment at once

```bash
kubectl set image deployments \
    -l type=db \
    db=mongo:3.4 --record
```



### Update the replicas in a deployment

```
kubectl scale deployment \
    go-demo-2-api --replicas 8 --record
```

