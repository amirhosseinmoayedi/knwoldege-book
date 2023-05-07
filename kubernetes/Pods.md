#kubenetes #k8S #pods
# <font color="#92d050">Pods</font>
Pods are <u>smallest deployable unit in kubernetes</u>.
Pods are <u>1 or more container deployed toghteher</u> with <font color="#fac08f">volume</font>, <font color="#fac08f">ip address </font>and they are <font color="#fac08f">disposable</font>.

> pods are better created via controller not directly


![[photo_2023-03-23_21-22-41.jpg]]
## Pods can be multiple container:  
1. <font color="#d99694">Main</font> Container : main application
2. <font color="#d99694">Init</font> Container which is runned before main ( Optional )
3. <font color="#d99694">side</font> containers which are to support the main one for example a proxy for main app. ( Optional )

## We can create pod or anything in kubernetes via two way:
1. <font color="#ffff00">Imperative</font> : Direct commands which is best used for <u>troubleshooting and learning</u>.
`kubctl run pod`
2. <font color="#ffc000">Declarative</font> : <u>best practices and reporduceable</u>.
`kubtcl apply -f declearitive_file.yaml`

Sample declarative config file:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: my-image:latest
    ports:
    - containerPort: 8080
    env:
    - name: MY_ENV_VAR
      value: "my value"
  restartPolicy: Always
```
