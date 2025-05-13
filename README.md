minikube start
minikube stop
minikube delete

# Build images
docker build -t backend-image:latest ./backend
docker build -t frontend-image:latest ./frontend
docker build -t mysql-database:latest ./database

# Load them into Minikube (if using Minikube)
minikube image load backend-image:latest
minikube image load frontend-image:latest
minikube image load mysql-database:latest

minikube image ls

# Apply Kubernetes Configurations
kubectl apply -f database/mysql-deployment.yaml
kubectl apply -f backend/backend-deployment.yaml
kubectl apply -f frontend/frontend-deployment.yaml

kubectl apply -f yaml/namespace.yaml
kubectl apply -f yaml/

minikube ip
kubectl get services/(svc)
kubectl get pods
kubectl get deployments
kubectl get nodes

1. 
kubectl delete service frontend
2. 
kubectl delete deployment frontend

kubectl port-forward service/frontend 8080:3000

minikube service frontend --url

kubectl exec -it frontend-58cbfd96dd-xkgzq -- sh -c "ss -tulnp || echo 'ss not found'"

# When pods are in different namespace
kubectl get deployments -n data-app

# When using the frontend in the browser,  the frontend code runs in your browser hence it is unable to access the backend
kubectl exec -it frontend-58cbfd96dd-kgh5j -n data-app -- curl -X POST \
  http://backend:5000/submit \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "address": "123 Main St"}'

kubectl exec -it frontend-58cbfd96dd-kgh5j -n data-app -- curl http://backend:5000/users

# Accessing Frontend on macOS with Minikube
Yes, this is likely related to how Minikube works specifically on macOS. On macOS, Minikube runs inside a VM or container (depending on your driver), which creates an additional networking layer that can cause issues with NodePort access.

# Why Direct NodePort Access Doesn't Work on macOS
On macOS, Minikube doesn't expose NodePorts directly to your host machine's network interface like it would on Linux. Instead, the NodePorts are exposed on the VM/container's network.