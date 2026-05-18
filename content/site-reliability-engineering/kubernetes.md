# kubectl Pod Commands — Study Notes

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