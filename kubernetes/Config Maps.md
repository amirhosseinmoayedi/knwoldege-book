#kubenetes #k8S 

<u>Same Image</u> should be used for dev, test, prod and etc enviroments., but the <u>configuration for enviroments changes</u>.

# <font color="#92cddc">ConfigMaps</font>
a map of<font color="#92cddc"> key-value that stores configuration</font>.
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
	name: myapp
data: # key-value pairs
	app-name: kube-test
	# multi line values
	html-sample: | 
		<h1> hello World </h1>
	app-version: 1.0.0
```

## we have two **ways of using config maps in pods**:
1. <font color="#e36c09">ENVs</font>: 
		on the start time of pod it will set enviroment variable(changes won't be reflected) 
2. <font color="#95b3d7">Volumes</font>:
		will create volume that contetn is directory structure that  for each key there is a file and the value of each key is in the file. 

<font color="#ffc000">Mount ConfigMaps as read-only volumes</font>: When you mount a ConfigMap as a volume, mount it as read-only <u>to prevent accidental modifications to the configuration data</u>. This will also ensure that all containers using the ConfigMap see the same configuration data.

Use ConfigMaps for secrets: **ConfigMaps can also be used to store sensitive data like passwords and API keys**. However,<u> it is recommended to use Kubernetes Secrets for sensitive data instead</u>, as they are encrypted at rest and in transit.

<font color="#ffc000">Use ConfigMap updates to trigger application restarts</font>: When you update a ConfigMap, <u>you can use Kubernetes to automatically trigger a rolling update </u>of your Pods to pick up the new configuration data.

![[Pasted image 20230402164725.png]]
![[Pasted image 20230402165603.png]]

Volume Example:
**Config Map**
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  app.config: |-
    option1=value1
    option2=value2
    option3=value3
  db.config: |-
    host=my-db-host
    port=5432
    username=my-db-user
    password=my-db-password
```
**Pod Deployment**
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
          image: my-image
          volumeMounts:
            - name: config-volume
              mountPath: /config
      volumes:
        - name: config-volume
          configMap:
            name: my-configmap

```

We can **map multiple configMap to a single directory**:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  volumes:
    - name: config-volume
      projected: # use this for unify the multiple configs or secrets 
        sources:
          - configMap:
              name: my-configmap
          - secret:
              name: my-secret
  containers:
    - name: my-container
      image: my-image
      volumeMounts:
        - name: config-volume
          mountPath: /config

```