# ConfigMaps and Secrets and RBAC

## Configs Demo

- Refresh minikube and create SC, PVC from before.

    `minikube delete --all`

- Config Maps commands

    ```
    // Get API
    kubectl api-resources | grep ConfigMap

    // Apply config map
    kubectl apply -f mongo-configmap.yaml

    // Get config maps
    kubectl get cm

    // Apply statefulsets
    kubectl apply -f statefulsets.yaml

    // Watch pods
    kubectl get pods

    // Enter one container
    kubectl exec -it mongo-0 -- bash

    env // Observe the values from config maps
    cat /etc/mongo/mongodb.conf 

    // Update config map
    kubectl apply -f mongo-configmap-1.yaml

    // Enter one container
    kubectl exec -it mongo-0 -- bash

    env // Observe the values from config maps its the same
    cat /etc/mongo/mongodb.conf 

    // Delete pod to refresh environment variables
    kubectl delete pod mongo-0

    // Enter one container
    kubectl exec -it mongo-0 -- bash

    env // Observe the values from config maps
    cat /etc/mongo/mongodb.conf 

    ```

## Secrets Demo

- Demo steps

    ```
    // Get the base64 value for password
    echo -n password | base64

    // Apply new config map
    kubectl apply -f mongo-configmap-2.yaml

    // Apply secret
    kubectl apply -f mong-secret.yaml

    //Apply statefulsets
    kubectl apply -f statefulsets-1.yaml

    // Enter one container
    kubectl exec -it mongo-0 -- bash

    env // Observe the values from config maps
    cat /etc/mongo/mongodb.conf 

    // Change password
    echo -n password123 | base64

    // Apply secret
    kubectl apply -f mong-secret-1.yaml

    // Delete pod to refresh environment variables
    kubectl delete pod mongo-0

    // Enter one container
    kubectl exec -it mongo-0 -- bash

    env // Observe the values from config maps
    cat /etc/mongo/mongodb.conf 

    // to decode
    echo -n cGFzc3dvcmQ= | base64 --decode
    ```

## RBAC

- User and namespace information is stored in kube/config folder. Follow below steps to add new user into kubernetes environment.

    ```
    nano ~/.kube/config

    // Use openssl to create PKI pair
    openssl genrsa -out keerthi.key 2048

    // Create a certificate signing request 
    openssl req -new -key keerthi.key -out keerthi.csr -subj "/CN=keerthi/O=dev/O=example.org"

    // View the files
    ls 

    // Get Certificate path from kube config
    nano ~/.kube/config

    // Create the certificate
    openssl x509 -req -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -days 730 -in keerthi.csr -out keerthi.crt

    // Set credentials
    kubectl config set-credentials keerthi --client-certificate=keerthi.crt --client-key=keerthi.key

    // Check the config to view new user added
    nano ~/.kube/config

    // Add the new user to a context
    kubectl config set-context keerthi-minikube --cluster=minikube --user=keerthi --namespace=default

    // Review updated config
    nano ~/.kube/config

    // You can also list the configs
    kubectl config get-contexts

    // Change to the user context
    kubectl config use-context keerthi-minikube

    // View the updated config
    kubectl config get-contexts

    // Try kubectl commands
    kubectl get pods

    // User do not have permissions yet.Change context to minikube again
    kubectl config use-context minikube

    // Apply roles and role bindings
    kubectl apply -f role.yaml
    kubectl apply -f role-binding.yaml

    // Change to the user context
    kubectl config use-context keerthi-minikube

    // Get pods
    kubectl get pods

    // Try creating a pod. Should fail because of permissions.
    kubectl run nginx --image=nginx

    // Change to minikube context 
    kubectl config use-context minikube

    // Create namespace
    kubectl create ns test

    // Run an image in the test namespace
    kubectl run nginx --image=nginx -n test

    // Get pods from test namespace
    kubectl get pods -n test

    // Create cluster role and cluster role binding
    kubectl apply -f cluster-role.yaml
    kubectl apply -f cluster-rolebinding.yaml

    // Change to the user context
    kubectl config use-context keerthi-minikube

    // Get pods
    kubectl get pods -n test

    ```

- Service accounts demo

    ```
    // Change to the user context
    kubectl config use-context minikube

    // Get service accounts
    kubectl get sa
    kubectl get sa -n test

    // Create service aaccount
    kubectl create sa test-sa

    // Change context
    kubectl config use-context keerthi-minikube

    // Create pod, exec and get pods. Forbidden error should be shown.  
    kubectl apply -f kubectl-pod.yaml
    kubectl exec -it kubectl-pod -- bash
    kubectl get pods

    // Change to the user context
    kubectl config use-context minikube

    // Apply new role binding with the service account
    kubectl apply -f role-binding-1.yaml

    // Delete and re-create pod, exec and get pods. 
    kubectl apply -f kubectl-pod-1.yaml
    kubectl exec -it kubectl-pod -- bash
    kubectl get pods

    ```

- To check permissions for a user you can use below commands.

    ```
    // Change context
    kubectl config use-context minikube

    // Check for creating pods
    kubectl auth can-i create pods

    // Check for creating pods on sa
    kubectl auth can-i create pods --as="system:serviceaccount:default::test-sa"
    ```