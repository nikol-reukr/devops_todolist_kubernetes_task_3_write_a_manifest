This document provides instructions on how to deploy the ToDo application manifests to a Kubernetes cluster and how to verify the installation.

---
# Docker Hub Repository:
https://hub.docker.com/repository/docker/nnnikol/todoapp/general

## 1. Applying Manifests

All Kubernetes manifests are located in the `infrastructure` directory. To ensure the environment is set up correctly, apply them in the following order:

1. Create the Namespace:
   ```bash
   kubectl apply -f .infrastructure/namespace.yml

2. Deploy the ToDo Application Pod:
    ```bash
    kubectl apply -f .infrastructure/todoapp-pod.yml

3. Deploy the BusyBox Testing Pod:
    ```bash
    kubectl apply -f .infrastructure/busybox.yml

## 2. Testing via Port-Forward

1. Run the port-forward command:
    ```bash
    kubectl port-forward todoapp-pod 8080:8000 -n todoapp   
   
2. Verify in your browser or via curl:
    
    Liveness: Open http://localhost:8080/api/health/live
    Readiness: Open http://localhost:8080/api/health/ready

## 3. Testing via busybox container

1. Retrieve the Pod's internal IP address:
   ```bash
   kubectl get pod todoapp-pod -n todoapp -o wide

2. Execute a curl command from the BusyBox pod:
   ```bash
   kubectl exec -it busybox -n todoapp -- curl http://<POD_IP>:8000/api/health/ready

Expected Result: - If called within the first 40 seconds, you will receive a 503 Service Unavailable ("Starting up...").

After 40 seconds, you will receive a 200 OK ("Ready to work!").


