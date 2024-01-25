# Monitoring and Logging

## Install Helm for Linux

- https://helm.sh/docs/intro/install/

    ```
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    chmod 700 get_helm.sh
    ./get_helm.sh
    ```

## Add helm repo

`helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`

## Update helm repo

`helm repo update`

## Install helm 

`helm install prometheus prometheus-community/prometheus`

## Check for Prometheus installation

`kubectl get pods`
`kubectl get svc`

## Metrics on Prometheus

- Kubernetes API exposes metrics already. But for additional metrics you can use PromQL. 
- Use Kube-state-metrics to get the metrics beyond what is being provided by API Server. 
- Expose prometheus server

`kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext`

`kubectl get svc`

- Get Minikube IP and go to the port prometheus server is running on. 

`minikube ip`

# Install Grafana

## Add helm repo

`helm repo add grafana https://grafana.github.io/helm-charts`

## Update helm repo

`helm repo update`

## Install helm 

`helm install grafana grafana/grafana`

## Expose Grafana Service

`kubectl expose service grafana --type=NodePort --target-port=3000  --name=grafana-ext`

## Get Grafana admin password

`kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
`

## Add Prometheus as data source for Grafana
## Import Dashboard from 3662 ID.
## Hover on a metric you will get the query. 

## Grafana Labs

List of Dashboards - https://grafana.com/grafana/dashboards/
Kube state metrics - https://grafana.com/grafana/dashboards/13332-kube-state-metrics-v2/ - 6417

## Configure Prometheus Server

```
kubectl get cm
kubectl edit cm prometheus-server
```

## PromQL Examples

```
    // Provides information about each pod including its namespace, name, IP address and status
    sum by (namespace) (kube_pod_info)

    // Calculates sum of changes in the ready status of Kubernetes pods over the last 5 minutes
    sum by (namespace)(changes(kube_pod_status_ready{condition="true"}[5m]))

    // Number of pods not in ready
    sum by (namespace) (kube_pod_status_ready{condition="false"})

    // Number of healthy clusters
    sum(kube_node_status_condition{condition="Ready", status="true"}==1)

```

## Adding Panel in Grafana

```
    // Add Panel 
    // Add Visualization 
    // Enter title CPU Usage
    // Under Metrics shift to code 
    100 - (avg by (instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

    //Save the panel and go back to dashboard

    // Try inducing stress 
    sudo apt-get install stress
    stress --cpu 2 --io 1 --vm 1 --vm-bytes 128M --timeout 40s

```