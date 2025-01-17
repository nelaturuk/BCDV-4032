# Demo for need of service for pods

```

kubectl apply -f nginx-deployment.yaml

Try accessing Pods directly:

kubectl get pods -o wide

curl <Pod-IP>

```

Observation: If you delete or restart a Pod (kubectl delete pod <pod-name>), its IP changes, breaking direct access.

Create a Service:

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: ClusterIP

```

Deploy the service

```
kubectl apply -f nginx-service.yaml

Access the service

kubectl get service nginx-service

curl <Service-IP>

```

Observation: Even if Pods are restarted or replaced, the Service IP remains consistent, providing stable access.

Expose the Service for external access: Change the Service type to NodePort or LoadBalancer

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: NodePort

```

Access it using the node's IP and allocated port.

