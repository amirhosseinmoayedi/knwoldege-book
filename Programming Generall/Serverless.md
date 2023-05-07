#architecture #general 

<font color="#e36c09">Serverless</font> is a <font color="#31859b">cloud-native</font> <u>development model</u> that allows developers to build and run applications <u>without having to manage servers</u>, they are <font color="#ffff00">stateless</font>.

A <font color="#31859b">cloud provider</font> handles the routine work of provisioning ( the action of providing or supplying something for use ), maintaining, and scaling the server infrastructure. Developers can simply package their code in containers( #docker) for deployment.

Serverless offerings from public cloud providers are usually metered on-demand through an [[event-driven]] execution model. **As a result, when a serverless function is sitting idle, it doesn’t cost anything**.

Serverless differs from other <font color="#31859b">cloud computing</font> models in that the cloud provider is responsible for **managing** <u>both the cloud infrastructure and the scaling of apps</u>.

Serverless computing offerings typically fall into two groups:
1. Backend-as-a-Service (<font color="#fac08f">BaaS</font>)
2. Function-as-a-Service (<font color="#fac08f">FaaS</font>)

### Backend-as-a-Service
BaaS gives developers access to a variety of third-party services and apps. For instance, a cloud-provider may offer authentication services, extra encryption, cloud-accessible databases, and high-fidelity usage data.
thery are called throw [[API]].
### Function-as-a-Service 
developers still write custom server-side logic, but it’s run in containers fully managed by a cloud services provider.

## Use case of Serverless
Serverless architecture is ideal for asynchronous, stateless apps that can be started instantaneously. Likewise, serverless is a good fit for use cases that see infrequent, unpredictable surges in demand.


serverless are deployed usually with Knative on [[Programming Generall/kubernetes]].
Knative is an open source community project which adds components for deploying, running, and managing serverless apps on Kubernetes.


![[Pasted image 20230304113737.png]]