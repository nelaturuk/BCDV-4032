```
// Get minikube addons list

minikube addons list


// Enable metrics addon if not done already

minikube addons enable metrics-server


// View pods and services installed

kubectl get pod,svc -n kube-system

// Get output from metrics server

kubectl top pods

// Dashboard for metrics
minikube dashboard

// API version for metrics
kubectl get --raw /apis/metrics.k8s.io/

// Get available resources
kubectl get --raw /apis/metrics.k8s.io/v1beta1

// Metrics API queries
kubectl get nodes
kubectl get --raw /apis/metrics.k8s.io/v1beta1/nodes/minikube
kubectl get --raw /apis/metrics.k8s.io/v1beta1/namespaces/<NAMESPACE>/pods/<Pod_NAME>

// Resource usage using top command
kubectl top --help
kubectl top node minikube
kubectl top kube-system pod metrics-server-<name> --namespace
kubectl top --namespace kube-system
kubectl top pod metrics-server-<name> --containers --namespace kube-system

// Resources allocated to node
kubectl describe node minikube




```