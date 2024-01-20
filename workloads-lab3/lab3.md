# Lab 3 Instructions

## Multinode Demo

- Start minikube with in multinode with below command:
    
    `minikube start --nodes 2 -p multinode-demo`

- Get list of nodes

    `kubectl get nodes`

- Check the status of nodes

    `minikube status -p multinode-demo`

- To view the dashboard

    `minikube dashboard -p multinode-demo`

- To stop nodes and clean up 

    ```
    minikube stop -p multinode-demo 
    minikube delete --all
    ```

## ReplicaSet Demo

- Run the replica set yaml file using apply command. 

    `kubectl apply -f replicaset.yaml`

- Get the pods observe the pods that got created 

    `kubectl get pods`

- Lets try deleting pod and see what happens. 

    ```
    kubectl delete pod nginx-<code>

    kubectl get pods

    kubectl get replicasets
    ```

## Creating replica set with Deployment object 

- Run the replica set yaml file using apply command. 

    `kubectl apply -f deploy-replica.yaml`

- Get the pods observe the pods that got created 

    `kubectl get pods`

- Get the deployments

    `kubectl get deployments`

- Check the rollout history 

    `kubectl rollout status deployment nginx-deployment-replica`

## Creating a stateful set

- Run the stateful sets yaml file using apply command. 

    `kubectl apply -f statefulsets.yaml`

- Get the pods observe the pods that got created 

    `kubectl get pods`

- Get the stateful sets

    `kubectl get statefulsets`


## Creating a Deamon set

- Run the daemon sets yaml file using apply command. 

    `kubectl apply -f daemonset.yaml`

- Get the pods observe the pods that got created 

    `kubectl get pods`

- Get the stateful sets

    `kubectl get daemonsets`

## Creating Deployment object with resource limit

- Run the replica set yaml file using apply command. 

    `kubectl apply -f deployment-resource.yaml`

- Get the pods observe the pods that got created 

    `kubectl get pods`

- Get the deployments

    `kubectl get deployments`


## Creating Deployment object with health checks

- Run the replica set yaml file using apply command. 

    `kubectl apply -f deployment-resource.yaml`

- Get the pods observe the pods that got created 

    `kubectl get pods`

- Get the deployments

    `kubectl get deployments`


