#namespace #k8S #kubenetes 
# <font color="#00b050">NameSpace</font>
To put it simply, a <font color="#00b050">namespace</font> is <font color="#00b050">a way to partition resources in a Kubernetes cluster</font>. It's <u>like a virtual cluster inside a physical cluster</u>. <u>Each namespace has its own set of resources</u>, including pods, services, and deployments, and is <font color="#00b050">isolated</font> from other namespaces.

It is used for several purposes:
1. **provide a way to organize resources and make them easier to manage**: For example, you might have a namespace for your production environment, another for your development environment
2. **provide a level of resource isolation**, Each <u>namespace has its own set of resource quotas</u>: such as the maximum number of pods or services that can be created.
3. **can be used to provide access control**. You can assign different roles and permissions to different namespaces.
 
 Kubernetes by default it will use <font color="#fbd5b5">default</font> namespace.
 ![[Pasted image 20230414173314.png]]

### <font color="#31859b">Create namespace</font>
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
```
	### how to use it in resources 
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
...
```

## Cross NS Communication
For commiunication between the Namespaces we simpliy explecitly call the resource with nameSpace name
![[Pasted image 20230414180103.png]]