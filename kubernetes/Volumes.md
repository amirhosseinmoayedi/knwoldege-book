#volume #kubenetes #k8S 

# Volumes
Volumes are ways to store data, persitant or not.

## <font color="#fac08f">Emptydir</font>
Emptydir an<font color="#fac08f"> intialy empty volume</font> that is<font color="#fac08f"> used to share data inside a pod</font> because the life time of it is the same as pod , if pod dies it will die to.

If a container in a<u> Pod crashes</u>, the emptyDir <u>content is unaffected</u>.

```yaml
volumes:
  - name: example-volume
    emptyDir: {}
```
## <font color="#92cddc">Hostpath</font>
Hostpath is used when app need to <font color="#92cddc">access the host file system(node)</font>. because it is dangerous <font color="#92cddc">it usually read only</font> .Like mapping node `/var/log/` to the pods 
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
spec:
  containers:
    - name: test-container
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh", "-c", "while true; do echo hello; sleep 10; done" ]
      volumeMounts:
        - name: hostpath-volume
          mountPath: /data
  volumes:
    - name: hostpath-volume
      hostPath:
		type: DirectoryOrCreate
# It creates a directory if it doesn’t exist already.
	    readOnly: true # Add this line to make it read-only
        path: /mnt/data
```
## <font color="#548dd4">Persistent volume</font> 
<font color="#548dd4">Persist data beyond life of pod and does not matter if the pod moves to another node</font>, **it has plugins which cloud providers used for custom implementation**. 

![[Screenshot_2023-03-29-11-04-13-063_com.mxtech.videoplayer.pro.jpg]]

This is how it works, kubernetes provide an<u> interface called CSI(container storage interface</u>) which the <u>plugin or cloud provider should implement</u> and the <u>result is called peraistant volume (pv)</u> which is the <u>mapping from actual data to kubernetes</u>, if a<u> pod wants to consume the data it needes persistent volume claim (pvc)</u> 

### <font color="#c3d69b"> Persistent volume subsystem</font>
is an <font color="#c3d69b">api that abstarct implementation of storage from using it</font> via <u>persistent volume</u> and <u>persistent volume claim</u> 

![[Screenshot_2023-03-29-11-17-19-101_com.mxtech.videoplayer.pro.jpg]]
We can configure the volume from storage class which the pods will use the volume from storage classes throw policy it will set the claim policy and setting for volume.
Pods can make claim base on free storage left on persistent volume 

![[Pasted image 20230402000617.png]]
### <font color="#b2a2c7">Local</font>
**persitent on the local node**. 

<font color="#ffff00">Persitent</font>
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-local
spec:
  capacity:
    storage: 1Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce #  it can be mounted by one node at a time for read-write access.
  persistenceVolumeRecalimePolicy: Recyle
  storageClassName: manule
  hostPath:
    path: "/mnt/data"
```

-   `persistenceVolumeRecalimePolicy`: The policy for reclaiming the volume when it is no longer needed. In this case, it’s set to Recycle which performs a basic scrub ( rm -rf /thevolume/*) on the volume and makes it available again for a new claim.
-   `storageClassName`: The name of the StorageClass that should be used to provision this volume.
-   `hostPath`: The path on the host machine where the volume should be mounted.

<font color="#ffff00">Claim</font> 
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv-local
spec:
  resources:
	request:
		storage: 1Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
```

for using the claim by the pod
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
        volumeMounts:
        - name: nginx-persistent-storage # we use the name of volume that defined in the spec not the claim directly
          mountPath: /data/nginx
      volumes:
      - name: nginx-persistent-storage
        persistentVolumeClaim:
          claimName: pv-local # this is important that match the claim 
```