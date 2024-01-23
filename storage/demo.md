# Volumes and Persistent Volumes

## Data store at container-level

- Deploy a mongodb object

    `kubectl apply -f deployment-1.yaml`

- Forward port

    `kubectl port-forward service/mongo-svc 32000:27017`

- Create a DB on Mongo Compass at localhost:32000 and Add a document. 

- Exec into the MongoDB Container

    ```
    kuebctl exec -it <pod-name> -- sh

    // Stop mongodb process

    ps aux
    kill <processID>

    kubectl get pods

    ```

- The container will restart. Check the state of DB. Everything is cleared. 

## Empty DIR

- Add empty dir to deployment yaml. Deploy the new yaml file. 

    `kubectl apply -f deployment-2.yaml`
Create a DB on Mongo Compass and Add a document. 
- Exec into the MongoDB Container

    ```
    kuebctl exec -it <pod-name> -- sh

    // Stop mongodb process

    ps aux
    kill <processID>

    kubectl get pods

    ```

- The container will restart. Check the state of DB. Everything remains same. 
- To see the empty dir storage location we should enter minikube container. 

    ```
    minikube ssh

    sudo ls /var/lib/kubelet/pods

    // Get the name of the pod folder in a different terminal

    kubectl get pods mongo-79687ddf99-nzrl6 -o yaml

    // or 

    kubectl get pods mongo-79687ddf99-nzrl6 -o jsonpath='{.metadata.uid}'

    // minikube ssh 

    sudo ls /var/lib/kubelet/pods/8abea306-df8e-4868-a98c-26d8340c607c/volumes/kubernetes.io~empty-dir/mongo-volume

    ```

- Delete the pod

    `kubectl delete pod <pod-name>`

- Check the data location on minikube ssh and mongo compass. Data is lost. 

    `sudo ls /var/lib/kubelet/pods/8abea306-df8e-4868-a98c-26d8340c607c/volumes/kubernetes.io~empty-dir/mongo-volume`

## Persistent Volumes

- To get the api-resources 

    `kuebctl api-resources | grep Persistent`

- Apply PV

    `kubectl apply -f pv.yaml`

- Get PV

    `kubectl get pv`

- Create persistent volume claims

    `kubectl apply -f pvc.yaml`

- Get PVC

    `kubectl get pvc`

- Check for PV Status 

    `kubectl get pv`

- Apply Deployment with PVC

    ```
    kubectl apply -f deployment-4.yaml

    // Check the status of pods

    kubectl get pods

    // Issue with created pods

    kubectl describe pod mongo-54f7c77856-p2znv

    // Create storage folder in minikube docker

    minikube ssh

    sudo mkdir /storage/data -p

    // delete pvc

    kubectl delete pvc mongo-pvc

    // check status

    kubectl get pvc

    // To remove you need to remove the pods

    kubectl delete -f deployment-4.yaml

    kubectl get pvc 
    kubectl get pv

    // Delete PV to change the status
    kubectl delete pv mongo-pv

    kubectl get pv

    // Apply again 

    kubectl apply -f pv.yaml
    kubectl apply -f pvc.yaml
    kubectl apply -f deployment-4.yaml
    kubectl get pods
    kubectl port-forward service/mongo-svc 32000:27017

    // Delete mongo pod after creating database
    kubectl delete pod mongo-54f7c77856-28kgw
    kubectl get pods

    ```

## Storage Class

- Get the API for storage class. Check out the namespace. 

    `kubectl api-resources | grep Storage`

- Apply SC

    `kubectl apply -f sc.yaml`

- Get SC

    ```
    kubectl get sc
    kubectl get pv
    ```

- Apply new PVC

    ```
    kubectl apply -f pvc-1.yaml

    // Get PV
    kubectl get pv
    ```

## StatefulSets Demo

- Get and apply StatefulSets. Remove any previous deployments. 

    ```
    kubectl api-resources | grep statefulset

    // Delete previous deployment
    kubectl delete -f deployment-4.yaml

    //Apply stateful sets
    kubectl apply -f statefulsets.yaml

    //Watch pods getting created
    kubectl get pods -w

    kubectl get pods

    // Scale more replicas
    kubectl scale sts mongo --replicas=7

    kubectl get pods -w

    // Scale down

    kubectl scale sts mongo --replicas=3

    kubectl get pods -w

    //try deleting pod
    kubectl delete pod mongo-0

    kubectl get pods

    //Observe the PVC
    kubectl get pvc

    // Check the volume name in pod
    kubectl describe pod mongo-0 | grep volume
    ```

- Create a headless service

    `kubectl apply -f headless-service.yaml`