# 🚢 Docker, Kubernetes, Microservices Interview Questions

**Docker:**

- **What is Docker?**
    
    Docker is a platform that enables developers to package, ship, and run applications in containers.
    
- **What is a container?**
    
    A container is a standardized unit of software that includes everything it needs to run: code, runtime, system tools, libraries, and settings.
    
- **What are the benefits of using Docker?**
    
    Docker provides portability, isolation, resource management, and repeatability for application deployments.
    
- **What is a Docker image?**
    
    A Docker image is a read-only template that contains the code and instructions for creating a container.
    
- **What is a Dockerfile?**
    
    A Dockerfile is a text file that contains instructions for building a Docker image.
    

**Kubernetes:**

- **What is Kubernetes?**
    
    Kubernetes is a container orchestration platform that automates the deployment, scaling, and management of containerized applications.
    
- **What are the main components of Kubernetes?**
    
    Kubernetes has a control plane (API server, etcd, controller manager, scheduler) and worker nodes (kubelet, container runtime).
    
- **What is a pod in Kubernetes?**
    
    A pod is the smallest deployable unit in Kubernetes, representing a single container or a group of containers running on a node.
    
- **What is a service in Kubernetes?**
    
    A service provides a stable IP address and DNS name to access pods within a cluster.
    
- **What is an ingress?**
    
    An ingress is a networking abstraction in Kubernetes that exposes services to external traffic, typically through a load balancer or an HTTP reverse proxy.
    
- **What are namespaces?**
    
    Namespaces allow you to organize and isolate resources within a Kubernetes cluster.
    
- **What is a deployment?**
    
    A deployment allows you to define the desired state of your application and Kubernetes will automatically manage the pods to achieve that state.
    
- **What is a StatefulSet?**
    
    A StatefulSet is a deployment strategy for applications that require stable and persistent storage, such as databases.
    
- **What is a DaemonSet?**
    
    A DaemonSet ensures that a pod is running on every node in the cluster.
    
- **What is a ConfigMap?**
    
    A ConfigMap allows you to store non-secret configuration data and inject it into your containers.
    
- **What are secrets?**
    
    Secrets allow you to store sensitive information, such as API keys and passwords, and inject it into your containers.
    

**Microservices:**

- **What is a microservice?**
    
    A microservice is a small, independently deployable application that focuses on a specific business function.
    
- **What are the benefits of using microservices?**
    
    Microservices offer scalability, independent deployment, fault tolerance, and technology diversity.
    
- **What are the challenges of using microservices?**
    
    Microservices can be more complex to manage, require distributed systems knowledge, and can have increased communication overhead.
    
- **How do microservices communicate with each other?**
    
    Microservices can communicate through REST APIs, gRPC, or message queues.
    
- **What is service discovery?**
    
    Service discovery is the process of finding and accessing other microservices in a cluster.
    
- **What is an API Gateway?**
    
    An API Gateway is a single entry point for all client interactions with a microservices-based application.
    
- **What is the Circuit Breaker pattern?**
    
    The Circuit Breaker pattern helps to prevent cascading failures in microservices by isolating failures and allowing services to recover.
    
- **What is eventual consistency?**
    
    Eventual consistency means that data will eventually be consistent across all microservices, even if there are temporary discrepancies.
    
- **What is a distributed transaction?**
    
    A distributed transaction is a transaction that spans multiple services and databases.
    
- **What is the Saga pattern?**
    
    The Saga pattern is used for managing distributed transactions by breaking them down into smaller transactions that can be independently managed


## 🐳 Docker Commands

| Command                               | Description                                         |
|---------------------------------------|-----------------------------------------------------|
| `docker build -t name .`              | Build image from Dockerfile in current directory   |
| `docker images`                       | List local images                                  |
| `docker ps`                           | List running containers                            |
| `docker ps -a`                        | List all containers (including stopped)            |
| `docker run -it image`                | Run a container interactively                      |
| `docker run -d -p 8080:80 image`      | Run in detached mode and map ports                 |
| `docker exec -it container bash`      | Access shell of a running container                |
| `docker logs container`              | View logs of a container                           |
| `docker stop container`              | Stop a running container                           |
| `docker rm container`                | Remove a stopped container                         |
| `docker rmi image`                   | Remove an image                                    |
| `docker-compose up`                  | Start services defined in `docker-compose.yml`     |
| `docker-compose down`                | Stop and remove all containers in the compose file |

---

## ☸️ Kubernetes Commands

| Command                                        | Description                                             |
|------------------------------------------------|---------------------------------------------------------|
| `kubectl get pods`                             | List all pods in the current namespace                  |
| `kubectl get svc`                              | List services                                           |
| `kubectl get deployments`                      | List deployments                                        |
| `kubectl describe pod <pod-name>`              | Detailed info about a specific pod                     |
| `kubectl logs <pod-name>`                      | View logs of a pod                                      |
| `kubectl exec -it <pod-name> -- bash`          | Access a pod's shell                                    |
| `kubectl apply -f <file>.yaml`                 | Apply configuration from a YAML file                   |
| `kubectl delete -f <file>.yaml`                | Delete resource(s) defined in the YAML file            |
| `kubectl scale deployment <name> --replicas=n` | Scale deployment to n replicas                         |
| `kubectl port-forward pod/<pod> 8080:80`       | Forward pod port to localhost                          |
| `kubectl get nodes`                            | Show all cluster nodes                                 |
| `kubectl config get-contexts`                  | List all configured contexts                           |
| `kubectl config use-context <context>`         | Switch to a different Kubernetes context               |

---

### ✅ Tip for Interview or Daily Use
> Use `kubectl explain <resource>` or `kubectl --help` for built-in help and documentation.


## 🔄 Kubernetes Update Methods

Kubernetes offers multiple ways to **update applications and configurations**. These updates can be applied to deployments, pods, configmaps, secrets, etc.

---

### 📦 1. Rolling Update (Default for Deployments)
- **Description**: Updates pods **gradually**, minimizing downtime.
- **Command**:
  ```bash
  kubectl set image deployment/<deployment-name> <container-name>=<new-image>
  ```
- **Use Case**: Updating container image versions in a live app.

---

### 📝 2. `kubectl apply -f <file>.yaml`
- **Description**: Applies changes from a YAML manifest.
- **Command**:
  ```bash
  kubectl apply -f deployment.yaml
  ```
- **Use Case**: Updating deployments, configmaps, services, etc.

---

### ⚙️ 3. Edit Resources Inline
- **Command**:
  ```bash
  kubectl edit deployment <deployment-name>
  ```
- **Use Case**: Quick manual changes in live cluster (e.g. env vars, replicas).

---

### 🔄 4. Scale Manually
- **Command**:
  ```bash
  kubectl scale deployment <name> --replicas=5
  ```
- **Use Case**: Scale up/down your app without changing the YAML.

---

### 🧪 5. Recreate Strategy
- **Description**: Shuts down old pods **before** creating new ones.
- **YAML Snippet**:
  ```yaml
  strategy:
    type: Recreate
  ```
- **Use Case**: When you can’t run old and new pods simultaneously (e.g. DB migrations).

---

### ♻️ 6. Delete + Re-apply
- **Command**:
  ```bash
  kubectl delete -f app.yaml
  kubectl apply -f app.yaml
  ```
- **Use Case**: Full reset, useful for dev/test environments.

---

### ✅ Bonus Tips

- Check rollout status:
  ```bash
  kubectl rollout status deployment/<deployment-name>
  ```

- Roll back:
  ```bash
  kubectl rollout undo deployment/<deployment-name>
  ```

- View history:
  ```bash
  kubectl rollout history deployment/<deployment-name>
  ```
