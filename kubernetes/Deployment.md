#deployment #kubenetes #k8S 
# <font color="#92cddc">Deployment</font>
Deployment <u>create one or more</u> <font color="#92d050">replicaset</font> and maintaine **high-availability**, <font color="#92d050">Replicasets</font> <u>ensure correct number of pods always running</u> with usage of <font color="#fac08f">control loop</font> that check this in background. 

Pods of a replicaset <u>may run on different nodes</u>.

pods have their **name second part always random** like the replicasets because:
* <font color="#b2a2c7">Pod</font>: can have multile replicas
* <font color="#b2a2c7">Replicaset</font>: can have multiple version, for rolling update or A/B testing.

> If a resource is created by something else like a pod created by deployment, never delete the resource delete the manager in this case deployment. 

To specify **number of replicas**:
```yaml
Spec:
 replicas:3
```

`port-forward` is **only for testing and lerarning porpouse**, try to not use in production.

## <font color="#d99694">Deployment strategies</font>
we have different kind of deployment strategies, two of them are <u>supported by default in kubernetes</u>:
1. <font color="#95b3d7">Recreate</font>:
		Will drop the old instances, and create new one, it has down time and suitable in senario which your app can't have multiple version running at same time, like when new one has different data schema and must apply a migration.  
2. <font color="#95b3d7">Rolling</font>: 
		Will create new instances and scale them up and scale down the old ons if new ones are considerd healty. good when you app can have multiple version and there is zero down time.

We have other type like <font color="#ffff00">Canary Relase</font> which very similar to <font color="#92cddc">Rolling</font> but if deployment timeout happens ( new container never be <font color="#92d050">healty</font> in the <font color="#92d050">timeout</font> specified-default is 10 minute) the deployment will be canceld.

<font color="#8064a2">A/B</font> testing can be done via both canary and Rolling.

we never create new deployment files we modify them and store them in git.

```yaml
Annotation
 Kubernetes.io/cause-change:"any name for revsion name"
```

In Rolling update kubernetes <u>won't delete the old replicaset for rollback</u>, Just delete the pods.

Kubernetes <u>will keep 10 deployment revision for rollback</u>, it's can be cahnge but default is more than enough

```yaml
strategy:
  type: Rolling
  rollingParams:
    updatePeriodSeconds: 1 
    intervalSeconds: 1 
    timeoutSeconds: 120 
    maxSurge: "20%" 
    maxUnavailable: "10%" 
    pre: {} 
    post: {}
```
  
1. The time to wait between individual pod updates. If unspecified, this value defaults to `1`.
2. The time to wait between polling the deployment status after update. If unspecified, this value defaults to `1`.
3. The time to wait for a scaling event before giving up. Optional; the default is `600`. Here, _giving up_ means automatically rolling back to the previous complete deployment.
4. `maxSurge` is optional and defaults to `25%` if not specified. See the information below the following procedure.
5. `maxUnavailable` is optional and defaults to `25%` if not specified. See the information below the following procedure.
6. `pre` and `post` are both lifecycle hooks.

#### Examples of availbility control
-   `maxUnavailable=0` and `maxSurge=20%` ensures full capacity is maintained during the update and rapid scale up.
-   `maxUnavailable=10%` and `maxSurge=0` performs an update using no extra capacity (an in-place update).    
-   `maxUnavailable=10%` and `maxSurge=10%` scales up and down quickly with some potential for capacity loss


```yaml
strategy:
  type: Recreate
  recreateParams: 
    pre: {} 
    mid: {}
    post: {}
```
1. `recreateParams` are optional.
2. `pre`, `mid`, and `post` are lifecycle hooks.


sample Deployemnt yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-image:latest
        ports:
        - containerPort: 8080
      restartPolicy: Always
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
```

The `metadata` section specifies the name of the deployment.

The `template` section defines the pod template for the deployment, including the container definition for the `my-container` container.

