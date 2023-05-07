#kubenetes #k8S 
# Secrets 
very similar to [[Config Maps]] but used for storing sensetive data.

<font color="#e5b9b7">impartive</font> : Secrets can be created manually using the `kubectl create secret` command.

To use a Secret in a Pod, you <u>can mount it as a volume or use it as environment variables</u>.

If see the secrets from outside of pod that using it we will see the hash version, like when using `kubectl descrie secret something` but inside the pod we will the actual value. 

It's important to note that while Secrets are encrypted at rest and in transit, <font color="#92d050">they are still vulnerable to attacks that could compromise their data</font>. Therefore, you should always <font color="#92d050">follow best practices</font> for securing your Kubernetes clusters and applications, <font color="#92d050">such as using RBAC (Role-Based Access Control) to restrict access to Secrets and using tools like HashiCorp Vault to manage Secrets outside of Kubernetes</font>.

By "<font color="#ffff00">at rest</font>" we mean when the <font color="#ffff00">data is stored in a persistent storage location</font>, such as a disk or a database. In the case of Kubernetes Secrets, the <font color="#ffff00">data is stored encrypted in the Kubernetes etcd store</font>, which is the primary storage mechanism used by Kubernetes to store cluster configuration data.

By "<font color="#e36c09">in transit</font>" we mean when the <font color="#e36c09">data is being transmitted over a network</font>, such as when a client application sends a request to a server over HTTP or when a Pod running in a Kubernetes cluster communicates with another Pod.

In Kubernetes,<font color="#e5b9b7"> the algorithm used for hashing in Secrets is SHA-256</font>. SHA-256 is a widely-used cryptographic hash function that produces a 256-bit hash value.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: dXNlcm5hbWU=
  password: cGFzc3dvcmQ=

```
[secrets types](https://kubernetes.io/docs/concepts/configuration/secret/#secret-types)
* the `type` field **specifies the type of the Secret**, which in this case is `Opaque`, meaning <u>that the data is stored as an opaque byte array.</u> (it means that the data is stored as a **sequence of bytes that have no inherent meaning or structure** )
* The `data` field specifies the actual data that is stored in the Secret, and the data is encoded in <font color="#e5b9b7">base64 format</font>.

### base64
Base64 encoding<font color="#ffff00"> is not a form of encryption</font> and <font color="#ffff00">does not provide any inherent security beyond obfuscating the data</font>. Anyone who has access to the encoded data can easily decode it back to its original form. Therefore, Base64 encoding should not be <font color="#ffff00">relied upon for securing sensitive data on its own</font>.

> we use <font color="#00b050">Valut</font> for storing the secrets becasue of this.

### usage as volume
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
      volumeMounts:
        - name: my-secret-volume
          mountPath: /etc/my-app/secret
          readOnly: true
  volumes:
    - name: my-secret-volume
      secret:
        secretName: my-secret

```

### as ENV
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
      env:
        - name: USERNAME
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: username
        - name: PASSWORD
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: password

```

You can create an **immutable** Secret by setting the *immutable field* to true. For example,
```yaml
apiVersion: v1
kind: Secret
metadata:
  ...
data:
  ...
immutable: true 
```