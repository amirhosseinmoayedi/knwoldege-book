#commands #kubenetes #k8S #minikube

## <font color="#92cddc">MiniKube</font>
| Command | Description |
|---|---|
|start| start a cluster `--nodes` for specifying number of node if we want **more than just he master**|
|stop| stop the cluster |
|status| get the minikube status |
|run| |
|logs| get the cluster logs `--node` for specifying the node to get `-f` get to tail of logs |
|ip| get the ip node `--node` for getting ip of specific node|
|ssh| ssh into the node |
|delete| delete the cluster |

## <font color="#92d050">Kubectl</font>
| Command | Description |
|---|---|
|get all|get all resources|
|get (Resource)|to get list of pods in default namespace `-A` to see in all namespaces `-n` to select the namespace `--selector` to select labels|
|port-forward|forward the port of pods, hostPort:ContainerPort|
|delete (resource)|delete resources |
|apply|to run declarative command `-f` to specify file |
|logs|to see the logs `c` to specify the container if pod has more than one container|
|exec|to execut command inside pod `-it` for interactive `-c` for selecting the container|
|describe (resource)|to get details of a resource like `pod` or `replicaset`,|
|Rollout|for managing deployment, `undo` to rollback, `status` to check status `history` to see the list of reviosions of resource like previous prevsion that deployed `pause` to pause a resource `resume` to rsume a paues resource| 
|api-resources| see all the resources availabe on kubernetes|
|create| is used to create a new resource in the cluster(will create a new resource even if a resource with the same name already exists.)|

### <font color="#e36c09">Resources</font>:
* Namespace or ns
* Pods or pod
* Deployment
* Node
* Replicaset 
* Service 
* Endpoints
* PersitentVolume (pv)
* PersitentVolumeClaim (pvc)
* ConfigMaps
* Secrets

Examples:
```bash
kubectl port-forward pod/hello-world 8080:80
Rollout undo deployment hello-world --to-revision=1
Kubectl get pods --selector="tier=backend, environment=test"
kubectl describe nginxConf #nginxConf is a config-map
kubectl create namespace test-env # create namespace
```

`-o` in some command like `describe` to get output, the value of output can be:  `yaml` or `wide`.

`Kubectl get endpoint`
Show list of <u>endpoint for the healty and matched pods</u>, if bot healty there is no use of showing the pod. 