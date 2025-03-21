# SOLUTION.md



# 🛠 Solution: Understanding Kubernetes Architecture & Deploying a Sample Pod

## 📌 Task Overview
In this task, we explore Kubernetes architecture, including control plane and worker node components, and deploy a simple NGINX Pod manually.

---

## 🚀 Kubernetes Architecture
### 🔹 Control Plane Components:
1. **API Server**: The front-end for Kubernetes that processes REST API requests.
2. **Scheduler**: Assigns workloads to appropriate nodes based on resource availability.
3. **Controller Manager**: Maintains cluster state, managing controllers like Replication and Node controllers.
4. **etcd**: A distributed key-value store used for cluster configuration and state persistence.
5. **Cloud Controller Manager**: Integrates Kubernetes with cloud provider APIs for networking, storage, and scaling.

### 🔹 Worker Node Components:
1. **Kubelet**: Runs on each node, communicates with the API Server, and ensures containers are running.
2. **Container Runtime**: (e.g., Docker, containerd) Responsible for running containers.
3. **Kube Proxy**: Maintains network rules to allow service communication between nodes.

---

## 🚀 Deploying a Sample Pod
### 📌 YAML Configuration (`pod.yaml`):
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```
### 📌 Explanation:
- **apiVersion: v1** → Uses Kubernetes core API version.
- **kind: Pod** → Defines this resource as a Pod.
- **metadata** → Provides a unique name (`nginx-pod`) and labels (`app: nginx`).
- **spec** → Defines Pod specifications, including:
  - **containers** → List of containers inside the Pod.
  - **name: nginx** → Names the container.
  - **image: nginx:latest** → Uses the latest NGINX image from Docker Hub.
  - **ports** → Exposes port `80` for web access.

### 📌 Deploying the Pod:
```bash
kubectl apply -f pod.yaml
```
### 📌 Verify Deployment:
```bash
kubectl get pods
kubectl describe pod nginx-pod
```

---

## 📌 Interview Questions & Answers
### 1️⃣ **Can you explain how the Kubernetes control plane components work together and the role of etcd in this architecture?**
- The **API Server** acts as the entry point for all Kubernetes requests.
- The **Scheduler** assigns workloads to nodes based on resource availability.
- The **Controller Manager** ensures desired state management (e.g., ensuring the required number of Pods are running).
- **etcd** stores cluster data, including node states, configurations, and deployed applications. If `etcd` fails, cluster state retrieval and updates become impossible, potentially causing failures.

### 2️⃣ **If a Pod fails to start, what steps would you take to diagnose the issue?**
1. **Check Pod status**:
   ```bash
   kubectl get pods
   ```
2. **Describe Pod details**:
   ```bash
   kubectl describe pod <pod-name>
   ```
3. **Check logs for errors**:
   ```bash
   kubectl logs <pod-name>
   ```
4. **Debug with an interactive shell** (if the container is running but misbehaving):
   ```bash
   kubectl exec -it <pod-name> -- /bin/sh
   ```
5. **Check events for failures** (e.g., ImagePullBackOff, CrashLoopBackOff):
   ```bash
   kubectl get events
   ```

---

## 🎯 Conclusion for Task 1 :
By following these steps, we have successfully explored Kubernetes architecture and deployed a simple Pod. Troubleshooting techniques help diagnose and fix common Kubernetes issues.


# Solution for Task 2: Deploy and Manage Core Kubernetes Objects

## 🏗️ Kubernetes Core Objects for SpringBoot BankApp
This document details the deployment of core Kubernetes objects such as Namespaces, Deployments, ReplicaSets, StatefulSets, and DaemonSets for the **SpringBoot BankApp** application.

---

## 1️⃣ Create a Namespace
Namespaces help isolate resources within a Kubernetes cluster.

### `namespace.yaml`
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: bankapp
```

**Apply the Namespace:**
```bash
kubectl apply -f namespace.yaml
```

---

## 2️⃣ Deploy a Deployment
Deployments manage a ReplicaSet that ensures the desired number of Pods are running.

### `deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bankapp-deployment
  namespace: bankapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: bankapp
  template:
    metadata:
      labels:
        app: bankapp
    spec:
      containers:
      - name: bankapp-container
        image: myrepo/bankapp:latest
        ports:
        - containerPort: 8080
```

**Apply the Deployment:**
```bash
kubectl apply -f deployment.yaml
```

**Verify the ReplicaSet:**
```bash
kubectl get replicaset -n bankapp
```

---

## 3️⃣ Deploy a StatefulSet
StatefulSets are used for stateful applications such as databases.

### `statefulset.yaml`
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: bankapp-db
  namespace: bankapp
spec:
  serviceName: "bankapp-db"
  replicas: 2
  selector:
    matchLabels:
      app: bankapp-db
  template:
    metadata:
      labels:
        app: bankapp-db
    spec:
      containers:
      - name: bankapp-db-container
        image: postgres:latest
        ports:
        - containerPort: 5432
```

**Apply the StatefulSet:**
```bash
kubectl apply -f statefulset.yaml
```

---

## 4️⃣ Deploy a DaemonSet
DaemonSets ensure that a Pod runs on every node in the cluster.

### `daemonset.yaml`
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: bankapp-logger
  namespace: bankapp
spec:
  selector:
    matchLabels:
      app: bankapp-logger
  template:
    metadata:
      labels:
        app: bankapp-logger
    spec:
      containers:
      - name: bankapp-logger-container
        image: fluentd:latest
        ports:
        - containerPort: 24224
```

**Apply the DaemonSet:**
```bash
kubectl apply -f daemonset.yaml
```

**Verify DaemonSet:**
```bash
kubectl get daemonset -n bankapp
```

---

## 📌 Differences Between Deployment, StatefulSet, and DaemonSet

| Feature        | Deployment | StatefulSet | DaemonSet |
|---------------|------------|-------------|------------|
| **Pod Identity** | Random | Stable, ordinal index | Runs on all nodes |
| **Scaling** | Stateless, easy to scale | Maintains order & uniqueness | Runs on all available nodes |
| **Example Use Case** | Web apps, APIs | Databases, Kafka, Zookeeper | Logging, Monitoring |

---

## ❓ Interview Questions & Answers

### 1️⃣ How does a Deployment ensure the desired state of Pods is maintained?
A **Deployment** ensures that the desired number of replicas of a Pod are running at all times. It uses a **ReplicaSet** to manage these replicas. If a Pod fails, the ReplicaSet automatically replaces it to maintain the defined number of replicas.

### 2️⃣ Differences Between Deployment, StatefulSet, and DaemonSet & Example Scenarios
- **Deployment:** Best for stateless applications like web servers.
- **StatefulSet:** Ideal for stateful applications like databases (PostgreSQL, MongoDB).
- **DaemonSet:** Used for system-level tasks like log collection (Fluentd, Prometheus node exporter).

---

## ✅ Conclusion for task 2: 


## Task 3: Networking & Exposure – Create Services, Ingress, and Network Policies

### 📌 Scenario:
Expose your **SpringBoot BankApp** application to internal and external traffic by creating **Services** and configuring an **Ingress**, while using **Network Policies** to secure communication.

---

## ✅ Step 1: Create a Service
A **Service** is needed to expose the application inside or outside the cluster.

### **1.1 Service of Type `ClusterIP`** (Internal Traffic Only)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: bankapp-service
  namespace: bankapp-namespace
spec:
  selector:
    app: bankapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```
🔹 **Explanation:**
- `ClusterIP` is the default type and is only accessible within the cluster.
- Routes traffic to Pods labeled with `app: bankapp` on port **8080**.

### **1.2 Modify Service to `NodePort`** (External Access via NodePort)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: bankapp-service
  namespace: bankapp-namespace
spec:
  selector:
    app: bankapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30008
  type: NodePort
```
🔹 **Explanation:**
- **Exposes** the service externally via a **NodePort (30008)**.
- Any request to `NodeIP:30008` is forwarded to `bankapp` Pods.

### **1.3 Modify Service to `LoadBalancer`** (Cloud Load Balancing)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: bankapp-service
  namespace: bankapp-namespace
spec:
  selector:
    app: bankapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```
🔹 **Explanation:**
- Used in cloud environments (AWS, GCP, Azure) to expose the app using an external Load Balancer.
- It automatically assigns a **public IP address**.

---

## ✅ Step 2: Configure an Ingress
**Ingress** manages external access using domain-based routing.

### **Ingress YAML Configuration**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: bankapp-ingress
  namespace: bankapp-namespace
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: bankapp.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: bankapp-service
                port:
                  number: 80
```
🔹 **Explanation:**
- Routes `bankapp.example.com` requests to `bankapp-service`.
- Requires an **Ingress Controller** like **NGINX Ingress Controller**.

🚀 **Apply Ingress:**
```bash
kubectl apply -f ingress.yaml
```

---

## ✅ Step 3: Implement a Network Policy
A **NetworkPolicy** restricts Pod communication within a namespace.

### **Network Policy to Restrict Access**
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: restrict-bankapp-access
  namespace: bankapp-namespace
spec:
  podSelector:
    matchLabels:
      app: bankapp
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: allowed-app
      ports:
        - protocol: TCP
          port: 8080
```
🔹 **Explanation:**
- Only allows `allowed-app` Pods to connect to `bankapp` Pods on port `8080`.
- Enhances **security** by limiting communication.

🚀 **Apply Network Policy:**
```bash
kubectl apply -f network-policy.yaml
```

---

## 📌 Interview Questions & Answers

### ❓ Q1: How do `NodePort` and `LoadBalancer` Services differ in terms of exposure and use cases?
✅ **Answer:**
| Service Type | Exposure | Use Case |
|-------------|----------|----------|
| **ClusterIP** | Internal only | Default, inter-Pod communication |
| **NodePort** | Exposes app on a static port (e.g., `30008`) on each node | Direct access from external users |
| **LoadBalancer** | Provides a cloud provider's external IP | Best for production with cloud environments |

---

### ❓ Q2: What is the role of a `NetworkPolicy` in Kubernetes, and can you describe a scenario where it is essential?
✅ **Answer:**
- **NetworkPolicy** defines allowed **ingress/egress** traffic for Pods.
- **Scenario:**
  - **Without Policy:** Any Pod in the cluster can talk to `bankapp`.
  - **With Policy:** Only Pods with `allowed-app` label can access `bankapp`, improving **security**.

---

## 🎯 Conclusion for task 3
✅ Successfully deployed **Services, Ingress, and Network Policies** to expose and secure the **SpringBoot BankApp**.
✅ Implemented **ClusterIP, NodePort, and LoadBalancer**.
✅ Configured **Ingress** for domain-based routing.
✅ Applied **Network Policy** for secure traffic control.

# 🛠️ Task 4: Storage Management – Use Persistent Volumes and Claims

## 📌 Scenario
Deploy a component of the **SpringBoot BankApp** application that requires persistent storage by creating **Persistent Volumes (PV)**, **Persistent Volume Claims (PVC)**, and a **StorageClass** for dynamic provisioning.

---

## 🚀 Steps to Complete the Task

### 1️⃣ Create a Persistent Volume (PV)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: bankapp-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual-storage
  hostPath:
    path: "/mnt/data"
```

🔹 **Explanation:**
- `storage: 5Gi` → Allocates **5GB** of storage.
- `accessModes: ReadWriteOnce` → Allows one node to read/write at a time.
- `persistentVolumeReclaimPolicy: Retain` → Keeps data even if the PVC is deleted.
- `hostPath: /mnt/data` → Mounts a directory on the host machine.

---

### 2️⃣ Create a Persistent Volume Claim (PVC)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: bankapp-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: manual-storage
```

🔹 **Explanation:**
- Requests **5GB** storage from available PVs.
- Uses `ReadWriteOnce` mode, meaning one node can access it at a time.

---

### 3️⃣ Deploy an Application Using the PVC

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bankapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: bankapp
  template:
    metadata:
      labels:
        app: bankapp
    spec:
      containers:
        - name: bankapp-container
          image: springboot-bankapp:latest
          volumeMounts:
            - mountPath: "/data"
              name: bankapp-storage
      volumes:
        - name: bankapp-storage
          persistentVolumeClaim:
            claimName: bankapp-pvc
```

🔹 **Explanation:**
- The container runs `springboot-bankapp:latest`.
- Mounts `/data` directory using the PVC (`bankapp-pvc`).
- Ensures data persistence across Pod restarts.

---

## 📂 File Structure
```
.
├── pv.yaml   # Persistent Volume configuration
├── pvc.yaml  # Persistent Volume Claim configuration
└── deployment.yaml  # Deployment with mounted PVC
```

---

## 📝 Key Concepts & Explanations

### 🔹 How StorageClasses Facilitate Dynamic Provisioning
A **StorageClass** allows Kubernetes to dynamically create Persistent Volumes (PV) instead of manually defining them. It helps by:
- Automating volume creation on cloud providers (AWS EBS, GCE Persistent Disks, etc.).
- Enabling different storage types (SSD, HDD, etc.).
- Managing retention policies (`Retain`, `Delete`, `Recycle`).

Example of a dynamic StorageClass:
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: dynamic-storage
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
  fsType: ext4
```

---

## ❓ Interview Questions & Answers

**1️⃣ What are the main differences between a Persistent Volume (PV) and a Persistent Volume Claim (PVC)?**
   - **PV**: A **physical storage unit** provisioned by an admin.
   - **PVC**: A **request for storage** made by a Pod.
   - **Relation**: PVC binds to a PV to access storage.

**2️⃣ How does a StorageClass simplify storage management in Kubernetes?**
   - It **automates** volume creation.
   - Allows **dynamic provisioning** without pre-creating PVs.
   - Supports different **storage backends** (EBS, NFS, etc.).

---

## ✅ Conclusion task 4:
By following these steps, we have successfully configured **Persistent Storage** for the SpringBoot BankApp, ensuring data is not lost even when Pods restart. 🚀


## Task 5: Configuration & Secrets Management with ConfigMaps and Secrets

### ConfigMap
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: bankapp
  labels:
    app: bankapp
    component: backend
  data:
    DATABASE_HOST: "mysql-service"
    LOG_LEVEL: "debug"
```
### Secret
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
  namespace: bankapp
  labels:
    app: bankapp
  type: Opaque
  data:
    DB_USERNAME: "dXNlcm5hbWU=" # base64 encoded
    DB_PASSWORD: "cGFzc3dvcmQ="
```
### Application Deployment with ConfigMap & Secret
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bankapp-backend
  namespace: bankapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: bankapp
  template:
    metadata:
      labels:
        app: bankapp
    spec:
      containers:
        - name: backend
          image: bankapp-backend:v1
          env:
            - name: DATABASE_HOST
              valueFrom:
                configMapKeyRef:
                  name: app-config
                  key: DATABASE_HOST
            - name: DB_USERNAME
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: DB_USERNAME
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: DB_PASSWORD
```
## Task 6: Autoscaling & Resource Management

### Horizontal Pod Autoscaler (HPA)
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: bankapp-hpa
  namespace: bankapp
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: bankapp-backend
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```

## Task 7: Security & Access Control

### Role-Based Access Control (RBAC)
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: bankapp
  name: developer-role
rules:
- apiGroups: [""]
  resources: ["pods", "services"]
  verbs: ["get", "list", "create", "delete"]
```

### Role Binding
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: bankapp
  name: developer-rolebinding
subjects:
- kind: User
  name: developer
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer-role
  apiGroup: rbac.authorization.k8s.io
```

## Task 8: Job Scheduling & Custom Resources

### CronJob
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: db-backup
  namespace: bankapp
spec:
  schedule: "0 2 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: backup-image:v1
            command: ["/bin/sh", "-c", "backup-script.sh"]
          restartPolicy: OnFailure
```

## Task 9: Advanced Deployment with Helm

### Helm Chart Structure
```
helm-bankapp/
│── charts/
│── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│── values.yaml
│── Chart.yaml
```

This document provides YAML configurations, explanations, and best practices for Kubernetes tasks. 🚀

🚀 Kubernetes networking mastered! 🎉
By following these steps, we successfully deployed core Kubernetes objects to manage the SpringBoot BankApp efficiently within a Namespace. 🚀
🚀 **Next Steps:** Extend this setup with Deployments and Services to improve scalability and networking!

## 🚀 Deploy Online Shopping App Using Kubernetes and Helm

This solution outlines the steps required to deploy an online shopping application using **Kubernetes (K8S)** and **Helm**, covering prerequisites, deployment steps, and troubleshooting techniques.

---

## 🛠️ Tools and Technologies Used
- **Kubernetes (K8S)**
- **Helm**
- **Docker**
- **Docker Hub**
- **Git**

---

## 📋 Prerequisites
1. **Kubernetes Cluster** (Minikube, Kind, GKE, EKS, AKS, etc.)
2. **kubectl** installed
3. **Helm** installed
4. **Docker** installed
5. **Git** installed

---

## 🚀 Step-by-Step Guide

### 1. Install Helm

#### Option 1: Install Helm using Snap
```bash
sudo snap install helm --classic
```

#### Option 2: Install Helm using Package Manager
```bash
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

#### Verify Installation
```bash
helm version
```

---

### 2. Clone the Repository
```bash
git clone https://github.com/makeshchaudharyll/online_shopping_app.git
cd online_shopping_app
```

---

### 3. Containerize the Application

#### Create a Dockerfile
```Dockerfile
# Build stage
FROM node:20-alpine as build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage with Nginx
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

#### Build the Docker Image
```bash
docker build -t online_shopping_app:latest .
```

#### Tag and Push the Docker Image
```bash
docker tag online_shopping_app:latest <your-dockerhub-username>/online_shopping_app:latest
docker push <your-dockerhub-username>/online_shopping_app:latest
```

---

### 4. Create a Helm Chart

#### Create a Helm Chart
```bash
helm create online-shopping-app
```

#### Modify `values.yaml`
```yaml
replicaCount: 1

image:
  repository: <your-dockerhub-username>/online_shopping_app
  tag: latest
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: false
```

---

### 5. Deploy the Application Using Helm

#### Install the Helm Chart
```bash
helm install online-shopping-app ./online-shopping-app
```

#### Verify the Deployment
```bash
kubectl get pods
kubectl get services
```

#### Access the Application
```bash
kubectl port-forward svc/online-shopping-app 8080:80
```
Open in browser:
```
http://localhost:8080
```
![Online Shopping App](https://media.licdn.com/dms/image/v2/D4D12AQGGtafLJdgvZA/article-inline_image-shrink_1500_2232/B4DZVqD8Z.GkAY-/0/1741241175084?e=1747872000&v=beta&t=FeHPFKaBMVk3TpX___hXqN8UUTZxAfE0xkj2rirY6-c)

---

## 🔍 Troubleshooting

### 1. Check Kubernetes Logs
```bash
kubectl logs -f <pod-name>
```

### 2. Debugging Deployment Issues
```bash
kubectl describe pod <pod-name>
kubectl describe service <service-name>
kubectl get events --sort-by=.metadata.creationTimestamp
```

### 3. Verify Helm Release
```bash
helm list --all-namespaces
helm status online-shopping-app
```

### 4. Rollback Deployment
```bash
helm rollback online-shopping-app <revision-number>
```

---
## 📂 Repository Structure
```
online_shopping_app/
├── Dockerfile
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ...
├── helm/
│   ├── online-shopping-app/
│   │   ├── Chart.yaml
│   │   ├── values.yaml
│   │   └── templates/
│   │       ├── deployment.yaml
│   │       ├── service.yaml
│   │       └── ...
├── README.md
└── <application-code>/
```

---

## 🔒 Security Best Practices
- Use **RBAC** to restrict access in Kubernetes.
- Store sensitive data in **Kubernetes Secrets**.
- Regularly scan container images using **Trivy**.
- Implement **Pod Security Policies**.
- Use **Network Policies** to restrict traffic between services.

---

