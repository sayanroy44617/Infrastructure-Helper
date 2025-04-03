# Kubernetes Notes

## 1. Introduction to Kubernetes
Kubernetes is an open-source container orchestration platform that helps in managing containerized applications across multiple nodes.

### 1.1 Types of Nodes
- **Master Node:** Manages the cluster and consists of components like:
  - `kube-apiserver`
  - `etcd`
  - `controller manager`
  - `scheduler`
- **Worker Node:** Runs the actual application workload and consists of:
  - `kubelet`
  - `container runtime` (like Docker or containerd)
  - `kube-proxy`

### 1.2 Basic Kubernetes Commands
```sh
kubectl run nginx --image nginx # Deploy a pod
kubectl get pods  # Get running pods
kubectl create -f definition.yaml  # Create resources using YAML files
```

## 2. Kubernetes YAML File Structure
A Kubernetes YAML file consists of four top-level fields:

- **apiVersion:** Version of the Kubernetes API.
- **kind:** Type of object (Pod, ReplicaSet, Deployment, Service, etc.).
- **metadata:** Information about the object (name, labels, etc.).
- **spec:** Specification describing the desired state.

### 2.1 Example: Creating a Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
    - name: redis
      image: redis
```

---

## 3. ReplicaSet and Scaling
### 3.1 What is a ReplicaSet?
A **ReplicaSet** ensures that a specified number of pod replicas are running at any given time.

### 3.2 Scaling a ReplicaSet
There are two ways to scale the number of replicas:
```sh
kubectl replace -f replica-definition.yaml  # Modify the YAML and replace the resource
kubectl scale --replicas=6 -f replica-definition.yaml  # Scale via CLI
```

# Kubernetes ReplicaSet and Pod Example

## Pod Definition

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
  labels:
    name: my-app
    type: frontend
spec:
  containers:
    - name: nginx
      image: nginx
```

## ReplicaSet Definition

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    name: my-app
    type: frontend
spec:
  replicas: 3  # How many pods will be created
  selector:
    matchLabels:
      type: frontend  # Selector to manage matching pods
  template:
    metadata:
      name: my-app-pod
      labels:
        name: my-app
        type: frontend
    spec:
      containers:
        - name: nginx
          image: nginx
```

---

## Explanation

### **Pod Definition**

- Creates a standalone **Pod** named `my-app-pod`.
- Uses labels **name: my-app** and **type: frontend**.
- Runs an **nginx** container.

### **ReplicaSet Definition**

- Manages **3 replicas** of Pods.
- Uses **matchLabels** to control which Pods it manages.
- The **template** inside the ReplicaSet defines the **Pod structure** (same as above).

---

## Commands to Deploy

1. **Apply the Pod definition** (if running standalone):

   ```sh
   kubectl apply -f pod.yaml
   ```

2. **Apply the ReplicaSet definition**:

   ```sh
   kubectl apply -f replicaset.yaml
   ```

3. **Check running Pods**:

   ```sh
   kubectl get pods
   ```

4. **Check ReplicaSet status**:

   ```sh
   kubectl get replicaset
   ```



---

## 4. Deployments
A **Deployment** is a higher-level abstraction that manages ReplicaSets and provides features like rolling updates and rollbacks.

### 4.1 Deployment YAML Example
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: httpd-frontend
  template:
    metadata:
      labels:
        app: httpd-frontend
    spec:
      containers:
        - name: httpd-container
          image: httpd:2.4-alpine
```

### 4.2 Deployment Commands
```sh
kubectl create -f deployment.yaml  # Create a deployment
kubectl apply -f deployment.yaml  # Apply changes to an existing deployment
```

### 4.3 Rollout and Rollback
```sh
kubectl rollout status deployment/myapp-deployment  # Check rollout status
kubectl rollout history deployment/myapp-deployment  # View rollout history
kubectl rollout undo deployment/deployment_name  # Rollback to previous version
```

---

## 5. Kubernetes Services
Kubernetes Services allow communication between different pods or expose them outside the cluster.

### 5.1 Types of Services
1. **NodePort** - Exposes a service on a static port on each node.
2. **ClusterIP** - Internal communication within the cluster.
3. **LoadBalancer** - Exposes service externally using a cloud provider’s load balancer.

### 5.2 Service YAML Example (NodePort)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:
    app: my-app
    type: frontend
```

### 5.3 Get Service Details
```sh
kubectl describe services service-name
minikube service service-name --url  # Get URL for a service running in Minikube
```

---

## 6. Additional Kubernetes Concepts

### 6.1 ConfigMaps
**ConfigMaps** are used to pass configuration data to containers.
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config

data:
  database_url: mysql://db-user:password@mysql-service/dbname
```
```sh
kubectl create configmap app-config --from-literal=database_url=mysql://db-user:password@mysql-service/dbname
```

### 6.2 Secrets
**Secrets** store sensitive information like API keys, passwords, or certificates.
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret

type: Opaque
data:
  password: cGFzc3dvcmQ=  # Base64 encoded "password"
```
```sh
kubectl create secret generic app-secret --from-literal=password=supersecret
```

### 6.3 Ingress
**Ingress** is an API object that manages external access to services.
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: myapp-service
            port:
              number: 80
```
```sh
kubectl apply -f ingress.yaml
kubectl get ingress
```

### 6.4 StatefulSets
Used for **stateful applications** like databases where each pod needs a unique identity.
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: "mysql"
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
```
```sh
kubectl apply -f statefulset.yaml
kubectl get statefulset
```

---

## 7. Summary
- **Pods** are the smallest deployable unit in Kubernetes.
- **ReplicaSets** ensure a certain number of pods are always running.
- **Deployments** manage ReplicaSets and enable rollouts/rollbacks.
- **Services** expose pods internally or externally.
- **ConfigMaps and Secrets** manage configuration and sensitive data.
- **Ingress** provides HTTP and HTTPS routing.
- **StatefulSets** manage stateful applications like databases.

---

This covers essential Kubernetes concepts and additional missing ones for better understanding. 🚀
