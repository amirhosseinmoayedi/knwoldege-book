#kubenetes #k8s #healthcheck

# HealtChek 
a health check is a mechanism for monitoring the health of a pod and ensuring that it's ready to handle requests.

Kubernetes provides two types of health checks: 
1. readiness probes
2. liveness probes

Both types of health checks can be configured using HTTP requests, TCP sockets, or command execution.
##  <font color="#92cddc">readiness probe</font>
A readiness probe is used to determine <font color="#92cddc">if a pod is ready to receive traffic</font>. If a pod <font color="#92cddc">fails its readiness probe</font>, Kubernetes<font color="#92cddc"> will remove it from the list of endpoints for its associated service</font>, and it <font color="#92cddc">won't receive any traffic until it passes the probe again</font>. This is useful for preventing traffic from being sent to pods that aren't fully initialized yet or are experiencing issues.

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
        readinessProbe:
          httpGet:
            path: /healthz
            port: 8080
          timeoutSeconds: 1
          failureThreshold: 3
          initialDelaySeconds: 10
          periodSeconds: 5
```

-   `initialDelaySeconds`: This field specifies how long to wait before running the first readiness check after the container starts. In this example, we're waiting 10 seconds before running the first check.
-   `periodSeconds`: This field specifies how often to run subsequent readiness checks. In this example, we're running checks every 5 seconds.
- `timeoutSeconds`: This field specifies the maximum amount of  time to wait for a response from the readiness check before considering it a failure. In this example, the timeout is set to 1 second.
- `failureThreshold`: This field specifies the number of consecutive failures that must occur before the container is considered not ready. In this example, we're set to fail after 3 consecutive failures.

### <font color="#ffc000">defaults</font> are:
-   `initialDelaySeconds`: 0
-   `periodSeconds`: 10
-   `timeoutSeconds`: 1
-   `successThreshold`: 1
-   `failureThreshold`: 3

##  <font color="#76923c">liveness probe</font>
A liveness probe is<font color="#76923c"> used to determine if a pod is still running and healthy</font>. <font color="#76923c">If a pod fails</font> its liveness probe, Kubernetes<font color="#76923c"> will restart the pod</font>. This can help ensure that your application stays up and running, even if individual pods fail.

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
        livenessProbe:
          httpGet:
            path: /healthz
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
```

