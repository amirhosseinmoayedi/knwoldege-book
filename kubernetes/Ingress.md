#kubenetes #k8S 
# <font color="#548dd4">Ingres</font>
Ingress<u> exposes </u>[[HTTP]]<u> and HTTPS routes from outside the cluster to </u> [[services]] <u>within the cluster</u>. Traffic routing is controlled by <u>rules defined on the Ingress resource</u>.
![[Pasted image 20230414230250.png]]
An Ingress may be configured to **give Services externally-reachable URLs**, **load balance traffic**, **terminate SSL / TLS**, and offer name-based virtual hosting.

acts as a **layer 7 (application layer) load balancer**, and allows you to <u>expose multiple services on a single IP address and port</u>.

Ingress can be used as an [[API Gateway]] in Kubernetes.

When you create an <font color="#548dd4">Ingress resource</font> in Kubernetes, you typically also need to deploy an <font color="#95b3d7">Ingress controller</font>, <u>which is responsible for implementing the rules defined in the Ingress resource</u>.

### Resource
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: mydomain.com
    http:
      paths:
      - path: /app1
        pathType: Prefix
        backend:
          service:
            name: app1-service
            port:
              name: http
      - path: /app2
        pathType: Prefix
        backend:
          service:
            name: app2-service
            port:
              name: http
```
The `pathType` field specifies **whether the path should match the beginning of the URL** (`Prefix`) or the **entire URL** (`Exact` or `ImplementationSpecific`).

##  <font color="#31859b">Ingress controllers</font>
There are many different Ingress controllers available for Kubernetes, each with its own set of features and capabilities. Some popular Ingress controllers include:

-   Nginx Ingress Controller
-   Traefik
-   Istio
-   HAProxy
-   Kong