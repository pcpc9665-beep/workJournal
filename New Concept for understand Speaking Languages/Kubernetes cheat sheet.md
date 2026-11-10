



| Name | Usage | Command |
| :--- | :--- | :--- |
| Install | Install Kubernetes (on Windows using Minikube) | `choco install minikube` |
| Run | Start Minikube cluster | `minikube start` |
| Version | Check Kubernetes version | `kubectl version --client` / `kubectl version --short` |
| Cluster Info | Show cluster information | `kubectl cluster-info` |
| Nodes | List cluster nodes | `kubectl get nodes` |
| Namespaces | List namespaces | `kubectl get namespaces` |
| Create NS | Create a namespace | `kubectl create namespace <name>` |
| Get Resources | List all resources in namespace | `kubectl get all -n <namespace>` |
| Pods | List pods | `kubectl get pods` |
| Create Pod | Create a pod from YAML | `kubectl apply -f <pod-file.yaml>` |
| Run Pod | Run a pod (quick way) | `kubectl run <pod-name> --image=<image>` |
| Delete Pod | Delete a pod | `kubectl delete pod <pod-name>` |
| Deployment | Create a deployment | `kubectl create deployment <name> --image=<image>` |
| List Deployments | List deployments | `kubectl get deployments` |
| Update Deployment | Update deployment image | `kubectl set image deployment/<name> <container>=<image>` |
| Rollback | Rollback deployment | `kubectl rollout undo deployment/<name>` |
| Delete Deployment | Delete a deployment | `kubectl delete deployment <name>` |
| Services | List services | `kubectl get services` |
| Create Service | Create a service from YAML | `kubectl apply -f <service.yaml>` |
| Ingress | List ingress resources | `kubectl get ingress` |
| Config Map | Create a config map | `kubectl create configmap <name> --from-file=<file>` |
| Secret | Create a secret | `kubectl create secret generic <name> --from-literal=key=value` |
| Logs | View pod logs | `kubectl logs <pod-name>` |
| Exec | Run a command in pod | `kubectl exec -it <pod-name> -- <command>` |
| Port Forward | Forward local port to pod | `kubectl port-forward <pod-name> 8080:80` |
| Describe | Get detailed information | `kubectl describe <resource> <name>` |
| Get YAML | Show resource YAML | `kubectl get <resource> <name> -o yaml` |
| Clean Up | Stop and delete Minikube | `minikube stop` / `minikube delete` |
