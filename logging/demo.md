# Logging Examples

## Print logs for a pod

```
// #Syntax
kubectl logs <pod_name>

// #Example
kubectl logs -n=kube-system kube-scheduler-controlplane
```

## Print the logs for the last 6 hours for a pod

```
// #Syntax
kubectl logs --since=6h <pod_name>

// #Example
kubectl logs --since=6h -n=kube-system kube-scheduler-controlplane
```

## Print the logs for a pod and follow for new logs

```
// #Syntax
kubectl logs -f <pod_name>

//#Example
kubectl logs -f -n=kube-system kube-scheduler-controlplane
```

## Print the logs for a container in a pod

```
/* Useful in the case of multi-container pods as it can be used to 
specify the name of container whose logs user really wants to see */

// #Syntax
kubectl logs -c <container_name> <pod_name>

//#Example
kubectl logs -c kube-scheduler -n=kube-system kube-scheduler-controlplane

```

## Output the logs for a pod into a file named ‘pod.log’

```
// #Syntax
kubectl logs <pod_name> > pod.log

//#Example
kubectl logs -n=kube-system kube-scheduler-controlplane > pod.log

```

## View the logs for a previously failed pod

```
// #Syntax
kubectl logs --previous <pod_name>

// #Example
kubectl logs --previous test-pod
```

## Get the most recent 10 lines of logs for a pod

```
// #Syntax
kubectl logs --tail=10 <pod_name>

// #Example
kubectl logs --tail=10 -n=kube-system kube-scheduler-controlplane
```

## View the logs for all containers in a pod

```
// #Syntax
kubectl logs <pod_name> --all-containers

// #Example
kubectl logs -n=kube-system kube-scheduler-controlplane --all-containers

```

## Grafana Loki and Promtail

```
helm repo add grafana https://grafana.github.io/helm-charts

helm repo list

helm repo update

helm show values grafana/loki-stack

// Create your own custom values file 'loki-stack-values.yaml':

# Enable Loki with persistence volume
loki:
enabled: true
size: 1Gi
promtail:
enabled: true
grafana:
enabled: true
sidecar:
 datasources:
   enabled: true
image:
 tag: 8.3.5


// Install Loki stack

helm install --create-namespace --namespace monitoring loki grafana/loki-stack --set filebeat.enabled=true,logstash.enabled=true,promtail.enabled=true --set fluent-bit.enabled=true,promtail.enabled=true --set grafana.enabled=true

// Get the grafana password
PASSWORD=$(kubectl get secret --namespace monitoring loki-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo); echo "${PASSWORD}"

// Port-forward grafana
kubectl port-forward --namespace monitoring service/loki-grafana 3000:80

// Add Loki Data source to grafana 
// Input http://loki:3100 in the url for data source 
// Go to Log browser 
// Query
{namespace="kube-system"}

```