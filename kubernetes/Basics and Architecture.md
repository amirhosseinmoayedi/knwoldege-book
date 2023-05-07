#kubenetes #k8S #architecture 

### Total Overview
![[Screenshot from 2022-04-29 15-02-27.png]]
<font color="#f79646">Control plane</font> makes desecion about cluster.
# <font color="#4bacc6">Kubernetes</font>
<font color="#92cddc">Kubernetes</font> or <font color="#92cddc">k8s</font> is an container orchestration platform with 
* Zero-delay
* Rollback
* Auto scale up/down.
Written in <font color="#fac08f">Golang</font>.

A kubernetes cluster consists of a <font color="#d99694">master</font> and several <font color="#d99694">workers</font>.
![[photo_2023-03-23_21-22-52.jpg]]

## <font color="#92d050">Master</font>
<u>manages all other cluster</u> and interact with cloud provider :
* <font color="#fac08f">API Server</font>: which is Resful API([[REST API]]) for <font color="#b2a2c7">internal</font> and <font color="#b2a2c7">external</font> communication over port 443, with <font color="#b2a2c7">authentication</font> and <font color="#b2a2c7">authorization</font>.
- <font color="#c3d69b">Cluster Store</font>:  wich is a <font color="#ffc000">etcd</font> ( a key-value disterbutes db) and stores the config.
* <font color="#ffff00">Scheduler</font>: which<font color="#ffc000"> watch for pods</font> , new pods and changes like being healthy, resource available and ...
* <font color="#95b3d7">Control Manager</font>: which consits of several managers that each check for current state and desired state and if not call the the correctanager node to fix it like the number of replicas
- <font color="#ffc000">Cloud Controller</font>: manager which is for <font color="#ffc000">interacting with underlying cloud provider</font> like azure and aws for instances , storages and load balancer

The worker node is a <u>vm or actual machine</u> that runs kubernetes.

## <font color="#d99694">Worker</font>
It's got three part:
* <font color="#f79646">kublet</font> : which it's job is to interact with master node and containter runtime.
![[photo_2023-03-23_21-22-56.jpg]]
- <font color="#92cddc">Runtime</font>: which is for pulling image, running and stop them.
- <font color="#92d050">kube proxy</font>: manages the network for each node like loadbalancing, assigning ip
### <font color="#ffff00">CRI</font>
The <u>Container Runtime Interface</u> (CRI) is the main protocol for the <u>communication between the kubelet and Container Runtime</u>, in [[GRPC]] format.

### <font color="#ffc000">Managed kubernetes</font>
Managed kubernetes means the master Nod is managed by someone else and we focous on worker node and containers. Examples are amazon manged service EKS.


### <font color="#92d050">Labels</font>
They are a way to categorize the pods in kubernetes, they can be anything we like, example:
```yaml
Environment: test 
Tier: backend 
```
<font color="#92d050">There purpose is to managing easier the pods</font> 

### <font color="#d99694">Anotaion</font>
Annotation is <font color="#d99694">unstructures key-value that it's key-value is set by external resource</font> and services and should be preserved when modifying object, and it's not queriable.


## <font color="#d99694">Resource Managment</font>
mangment of cpu and ram is called resource managment.
in kubernetes the we can specify the cpu and ram usage of our containers:
- <font color="#ffc000">Request</font>: **minimum** amount of resources a container needs.
- <font color="#ffff00">Limit</font>: **maximum** amount of resources a container can have.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: my-image:latest
    resources:
      requests:
        cpu: 0.5
        memory: 256Mi
      limits:
        cpu: 1
        memory: 512Mi
```

## <font color="#fbd5b5">Job</font>
a `Job` is a type of <font color="#fbd5b5">object that manages the execution of a batch process or a one-time task</font>. A `Job` creates one or more pods and <font color="#fbd5b5">ensures that they complete successfully</font>.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job
spec:
  template:
    spec:
      containers:
      - name: my-container
        image: my-image:latest
        command: ["/bin/bash"]
        args: ["-c", "echo 'Hello, world!'"]
      restartPolicy: Never
  backoffLimit: 3
```
 The `restartPolicy` is set to "Never", which means that the container won't be restarted if it fails.
 The `backoffLimit` is set to 3, which means that the `Job` will be retried up to 3 times if any of the pods fail.