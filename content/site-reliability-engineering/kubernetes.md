# kubectl Pod Commands — Study Notes

> Reading:
> - [Kubernetes Ingress with AWS ALB Ingress Controller](https://aws.amazon.com/blogs/opensource/kubernetes-ingress-aws-alb-ingress-controller/)

## Cluster & Resource Discovery
```bash
kubectl cluster-info                          # Show cluster endpoint info
kubectl get nodes                             # List all nodes and their status
kubectl api-resources                         # List all available resource types (pods, deployments, etc.)
kubectl api-resources --namespaced=true       # Only namespace-scoped resources
kubectl explain pod                           # Docs/schema for a resource type
kubectl explain pod.spec.containers           # Drill into a specific field
```

---

## Namespaces
```bash
kubectl get namespaces                        # List all namespaces
kubectl config set-context --current --namespace=<ns>   # Switch default namespace
```

---

## Creating Pods
```bash
# From an image (imperative)
kubectl run <pod-name> --image=nginx                          # Create and run a pod
kubectl run <pod-name> --image=nginx --port=80                # Expose a port
kubectl run <pod-name> --image=nginx --env="ENV=prod"         # Set env variable
kubectl run <pod-name> --image=nginx --labels="app=web"       # Attach labels

# From a YAML manifest (declarative — preferred in production)
kubectl apply -f pod.yaml                     # Create or update from file
kubectl apply -f ./manifests/                 # Apply all YAMLs in a directory
kubectl create -f pod.yaml                    # Create only (fails if already exists)
```

---

## Dry Run (Safe Testing)
```bash
kubectl run <pod-name> --image=nginx --dry-run=client         # Validate without creating
kubectl run <pod-name> --image=nginx --dry-run=client -o yaml # Generate YAML template
kubectl apply -f pod.yaml --dry-run=client                    # Test apply without changes
kubectl apply -f pod.yaml --dry-run=server                    # Server-side validation (more accurate)
```

> **Tip:** `--dry-run=client -o yaml > pod.yaml` is the fastest way to scaffold a pod manifest.

---

## Listing & Inspecting Resources
```bash
kubectl get pods                              # List pods in current namespace
kubectl get pods -A                           # List pods across all namespaces
kubectl get pods -o wide                      # Include node, IP, and other details
kubectl get pods -o yaml                      # Full YAML output
kubectl get pods -o json                      # Full JSON output
kubectl get pods --show-labels                # Show labels column
kubectl get pods -l app=web                   # Filter by label
kubectl get pods --field-selector=status.phase=Running   # Filter by field
kubectl get all                               # List pods, services, deployments, etc.
```

---

## Describing & Debugging
```bash
kubectl describe pod <pod-name>               # Full detail: events, conditions, mounts
kubectl logs <pod-name>                       # Pod logs (stdout)
kubectl logs <pod-name> --previous            # Logs from a crashed/restarted container
kubectl logs <pod-name> -c <container-name>   # Logs from a specific container (multi-container pods)
kubectl logs <pod-name> -f                    # Stream/follow live logs
kubectl events --for pod/<pod-name>           # Show events scoped to a pod (k8s 1.26+)
```

---

## Editing & Updating
```bash
kubectl edit pod <pod-name>                   # Open live config in $EDITOR (vi by default)
kubectl apply -f pod.yaml                     # Re-apply updated manifest (idempotent)
kubectl replace -f pod.yaml                   # Replace resource entirely (destructive)
kubectl patch pod <pod-name> -p '{"metadata":{"labels":{"env":"staging"}}}'   # Patch a field inline
kubectl label pod <pod-name> env=prod         # Add/update a label
kubectl label pod <pod-name> env-             # Remove a label (note the trailing dash)
kubectl annotate pod <pod-name> note="test"   # Add an annotation
```

---

## Executing Into Pods
```bash
kubectl exec <pod-name> -- ls /app                        # Run a one-off command
kubectl exec -it <pod-name> -- /bin/bash                  # Interactive shell
kubectl exec -it <pod-name> -c <container> -- /bin/sh     # Shell into a specific container
kubectl cp <pod-name>:/etc/config ./config                # Copy file out of pod
kubectl cp ./config <pod-name>:/etc/config                # Copy file into pod
```

---

## Port Forwarding
```bash
kubectl port-forward pod/<pod-name> 8080:80   # Forward localhost:8080 → pod port 80
```

---

## Deleting Pods
```bash
kubectl delete pod <pod-name>                 # Delete a pod
kubectl delete -f pod.yaml                    # Delete using manifest file
kubectl delete pod <pod-name> --grace-period=0 --force   # Force delete (stuck pods)
kubectl delete pods -l app=web                # Delete by label selector
```

---

## Config & Context
```bash
kubectl config view                           # Show kubeconfig (all contexts)
kubectl config get-contexts                   # List available contexts
kubectl config current-context               # Show active context
kubectl config use-context <context-name>     # Switch context (e.g. between clusters)
```

---

## Output Formatting Tips
```bash
-o yaml          # Full YAML output
-o json          # Full JSON output
-o wide          # Extra columns (node, IP)
-o name          # Just resource names
-o jsonpath='{.status.podIP}'   # Extract a specific field
--watch / -w     # Watch for live changes
```

---

> **Study Tip:** The flow for most tasks goes:  
> `explain` → `dry-run -o yaml` → edit YAML → `apply` → `get` → `describe` → `logs` → `exec`

# kubectl CKA Cheat Sheet — ReplicaSet, Deployment, Service, Namespace

---

## ReplicaSet
```bash
# Inspect
kubectl get rs                                # List all replicasets
kubectl get rs -o wide                        # Include selector and container info
kubectl describe rs <rs-name>                 # Full detail: desired/ready/available counts + events

# Create
kubectl apply -f replicaset.yaml              # Create from manifest (preferred)
kubectl create -f replicaset.yaml             # Create only (fails if exists)

# Scaling
kubectl scale rs <rs-name> --replicas=5       # Scale up/down directly
kubectl edit rs <rs-name>                     # Edit replicas or image in-place

# Delete
kubectl delete rs <rs-name>                   # Deletes RS and its pods
kubectl delete rs <rs-name> --cascade=orphan  # Delete RS but keep pods running
```

> **Note:** ReplicaSets are rarely managed directly — Deployments own them. Useful to know for CKA troubleshooting.

---

## Deployment
```bash
# Create
kubectl create deployment <name> --image=nginx                        # Imperative create
kubectl create deployment <name> --image=nginx --replicas=3           # With replica count
kubectl create deployment <name> --image=nginx --dry-run=client -o yaml  # Scaffold YAML
kubectl apply -f deployment.yaml                                      # Declarative create/update

# Inspect
kubectl get deployments                       # List deployments
kubectl get deploy -o wide                    # Include image, selector, availability
kubectl describe deployment <name>            # Full detail: strategy, replicas, events
kubectl get rs                                # See the replicasets a deployment manages

# Scaling
kubectl scale deployment <name> --replicas=5  # Scale pods
kubectl autoscale deployment <name> --min=2 --max=10 --cpu-percent=80  # HPA

# Rollout & Updates
kubectl set image deployment/<name> nginx=nginx:1.25         # Update container image
kubectl rollout status deployment/<name>                     # Watch rollout progress
kubectl rollout history deployment/<name>                    # View revision history
kubectl rollout history deployment/<name> --revision=2       # Inspect a specific revision
kubectl rollout undo deployment/<name>                       # Roll back to previous version
kubectl rollout undo deployment/<name> --to-revision=2       # Roll back to specific revision
kubectl rollout pause deployment/<name>                      # Pause rollout mid-way
kubectl rollout resume deployment/<name>                     # Resume paused rollout
kubectl rollout restart deployment/<name>                    # Force rolling restart (e.g. to pick up new secret)

# Edit & Patch
kubectl edit deployment <name>                               # Live edit in $EDITOR
kubectl patch deployment <name> -p '{"spec":{"replicas":3}}' # Inline patch

# Delete
kubectl delete deployment <name>              # Deletes deployment, RS, and pods
kubectl delete -f deployment.yaml
```

> **Tip:** `rollout undo` is a critical CKA command — know it cold.

---

## Service
```bash
# Create
kubectl expose pod <pod-name> --port=80 --target-port=8080            # Expose a pod
kubectl expose deployment <name> --port=80 --target-port=8080         # Expose a deployment
kubectl expose deployment <name> --type=NodePort --port=80            # NodePort (accessible outside cluster)
kubectl expose deployment <name> --type=LoadBalancer --port=80        # LoadBalancer (cloud only)
kubectl create service clusterip <name> --tcp=80:8080                 # ClusterIP via imperative
kubectl apply -f service.yaml                                         # From manifest

# Inspect
kubectl get svc                               # List services
kubectl get svc -o wide                       # Include selector and endpoints
kubectl describe svc <name>                   # Detail: endpoints, ports, selector
kubectl get endpoints <name>                  # Check which pod IPs the service routes to

# Dry Run / Scaffold
kubectl expose deployment <name> --port=80 --dry-run=client -o yaml   # Generate service YAML

# Delete
kubectl delete svc <name>
kubectl delete -f service.yaml
```

> **Service types to know for CKA:**  
> `ClusterIP` (default, internal only) → `NodePort` (static port on each node) → `LoadBalancer` (cloud LB) → `ExternalName` (DNS alias)

---

## Namespace
```bash
# Create
kubectl create namespace <name>               # Imperative
kubectl apply -f namespace.yaml               # From manifest

# Inspect
kubectl get namespaces                        # List all namespaces (alias: ns)
kubectl get ns
kubectl describe ns <name>                    # Detail: resource quotas, limits, status

# Working within a namespace
kubectl get pods -n <name>                    # List pods in a specific namespace
kubectl get all -n <name>                     # All resources in a namespace
kubectl get pods --all-namespaces             # Pods across every namespace
kubectl get pods -A                           # Same, shorthand

# Switch default namespace (so you don't type -n every time)
kubectl config set-context --current --namespace=<name>

# Apply a manifest into a namespace
kubectl apply -f pod.yaml -n <name>
kubectl run nginx --image=nginx -n <name>

# Delete
kubectl delete ns <name>                      # WARNING: deletes all resources inside it
```

> **CKA Tip:** Always check which namespace you're in before running commands. Many exam questions use non-default namespaces intentionally.

---

## Quick Reference — Resource Aliases

| Resource    | Short Alias |
|-------------|-------------|
| pods        | `po`        |
| replicasets | `rs`        |
| deployments | `deploy`    |
| services    | `svc`       |
| namespaces  | `ns`        |
| nodes       | `no`        |
| configmaps  | `cm`        |
| persistentvolumeclaims | `pvc` |

```bash
# Examples using aliases
kubectl get po -A
kubectl get deploy -n kube-system
kubectl get svc -o wide
```