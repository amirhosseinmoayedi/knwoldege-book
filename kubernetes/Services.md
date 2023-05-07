#services #kubenetes #k8S 
 
 Beacuse pods are <u>disposable and their ip is unreliable</u> and we may <u>scale up or down</u>( number of ip increases or may be unavailbe ) we can't use `port-forward` , we use <font color="#ffc000">services</font>.
 
# <font color="#ffc000">Services</font>
**Resources which their job is to access the pods via them**, have their <u>own ip address</u>, <u>DNS name</u> which is **constant**.
![[Screenshot_2023-03-26-13-16-09-378_com.mxtech.videoplayer.pro.jpg]]

We can check the **endpoints behind the services**(their adreess and port) by seeing the `describe` of service.

## There are 4 types of services:
- <font color="#95b3d7">Cluster IP</font> (<font color="#ffff00">defaults</font>)
		Internall access and no external
- <font color="#76923c">NodePort</font> 
		allow to open port on all nodes and will redirect from nodes to service
- <font color="#d99694">ExternalName</font>
- <font color="#31859b">LoadBalancer</font> 

Service sample <font color="#92cddc">cluster ip</font>
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: ClusterIP
  selector:
    app: my-app
  ports:
      port: 80
      targetPort: 8080 # port of pods
```

The `metadata` section provides information about the service, including its <font color="#c3d69b">name</font>.
The `selector` field <font color="#c3d69b">specifies the labels of the pods that the service should route traffic to</font>.

### <font color="#76923c">Node Port</font>
Allow to open port on all nodes , the <u>range is 30000 to 32767</u> , <u>client will call the node</u> then <u>node redirect mesaage to node port service</u> and serviec <u>will redirect it to pod</u> **wheter on that node that client sent request to or another node**.

<font color="#953734">Disadvantages</font> are:
* There can be one service per port 
* If Nodes ip changes the service won't work anymore 

![[photo_2023-03-26_14-37-00.jpg]]
![[photo_2023-03-26_14-36-55.jpg]]

sample yaml for <font color="#76923c">nodePort</font>
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
      port: 80
      targetPort: 8080 
      nodePort: 30001 # this optional if not set it will be generated from the specified range of node port.
```


### <font color="#92cddc">Load balancer</font>
Load balancer create a <font color="#92cddc">network loadbalancer per service</font> , which route traffic base on region, ip, etc, **the standard way of external expose**

> Each cloud provider has it own implementation of <font color="#c3d69b">Nlb</font> (netwrok load balancer) 

```yaml
apiVersion: v1
kind: Service
metadata:
  name: example-service
spec:
  selector:
    app: example
  ports:
    - port: 8765
      targetPort: 9376
      protocol: TCP
  type: LoadBalancer
```

<u>All comiunication goes throw the</u> <font color="#ffff00">kubernetes api server</font> <u>to nodes </u>.

## <font color="#b2a2c7">Service discovery</font>:
<font color="#b2a2c7">A way that service discover each other without knowing the ip</font>.

### <font color="#548dd4">Service registery</font>:
<font color="#fac08f">Core DNS</font> is used as service in kube-system namespace to keep the <font color="#548dd4">map of services name to service</font>.

> <u>Each pod</u> in kubernetes has `resolve.conf` file which <u>set the name server (DNS) to kube-dns(Core DNS) IP address(service registery)</u>.

For name resolution we have full adress which specify in this format:
`podname.namespace.resource-type.cluster.node` 

<font color="#ffff00">Kube proxy</font> implement controller that<font color="#ffff00"> watch the api server</font> for new service and endpoints  And is the <font color="#ffff00">reason for we can load balance between pods</font> .


## <font color="#00b0f0">Service Discovery</font>
Service discovery is a <u>mechanism by which services discover each other dynamically without the need for hard coding IP addresses or endpoint configuration</u>.

### <font color="#fac08f">Server-side service discovery</font>
involves <u>putting a load balancer (LB) in front of the service and letting the load balancer connect to service instances</u>.
![[Pasted image 20230414222541.png]]
the LB becomes a single point of failure and bottleneck. Additionally, the LB must implement service discovery logic to point to the correct instances of pods running at any point in time.

### <font color="#92cddc">Service registry</font>
The service registry contains information about service endpoints where clients can send requests.
![[Pasted image 20230414222813.png]]
The main advantage of a service registry compared to a server-side approach is that <u>there is one less component to manage (no LB)</u> and no bottleneck.

that a <u>service registry complicates the client-side logic</u>. The client must implement logic to keep the registry updated to ensure it contains the latest information about the backend pods/containers.
###  <font color="#548dd4">in kubernetes</font>
![[Pasted image 20230414222958.png]]
the Kubernetes control plane `ETCD` acts as a service registry where all the endpoints are registered and kept up to date by Kubernetes itself.