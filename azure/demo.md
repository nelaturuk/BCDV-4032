## Azure cluster demo

## Create cluster on AKS
## Install Azure CLI 

- Install using instructions [here](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)

- Run the below commands to create the cluster

```
    // Login to Azure
    az login

    // Copy and paster commands from the cluster connect
    az account set --subscription a8462f4c-ce9c-42bc-9d25-7ad2330a7489

    az aks get-credentials --resource-group aksdemo --name aksdemo --overwrite-existing

    // Check the config view
    kubectl config view

    // Get nodes 
    kubectl get nodes

    // Get all pods
    kubectl get pods -A
```

- Connect cluster to Lens