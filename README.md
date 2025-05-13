# 🐳 Minikube + Docker + Kubernetes Setup

## 🚀 Minikube Commands

```bash
minikube start         # Start Minikube
minikube stop          # Stop Minikube
minikube delete        # Delete Minikube cluster
```

---

## 💠 Build Docker Images

```bash
docker build -t backend-image:latest ./backend
docker build -t frontend-image:latest ./frontend
docker build -t mysql-database:latest ./database
```

---

## 📆 Load Images into Minikube (if using Minikube)

```bash
minikube image load backend-image:latest
minikube image load frontend-image:latest
minikube image load mysql-database:latest

minikube image ls      # List loaded images
```

---

## 📄 Apply Kubernetes Configurations

```bash
# Individual deployments
kubectl apply -f database/mysql-deployment.yaml
kubectl apply -f backend/backend-deployment.yaml
kubectl apply -f frontend/frontend-deployment.yaml

# Namespace and all other YAMLs
kubectl apply -f yaml/namespace.yaml
kubectl apply -f yaml/
```

---

## 📡 Service & Pod Management

```bash
minikube ip                          # Get Minikube IP
kubectl get services                 # List services
kubectl get pods                     # List pods
kubectl get deployments              # List deployments
kubectl get nodes                    # List nodes
```

### ♻️ Delete Specific Resources

```bash
kubectl delete service frontend      # Delete frontend service
kubectl delete deployment frontend   # Delete frontend deployment
```

### ↻ Port Forwarding

```bash
kubectl port-forward service/frontend 8080:3000
```

### 🌐 Access Service URL

```bash
minikube service frontend --url
```

### 🧲 Debugging Inside a Pod

```bash
kubectl exec -it frontend-58cbfd96dd-xkgzq -- sh -c "ss -tulnp || echo 'ss not found'"
```

---

## 📂 Working with Namespaces

```bash
# Get deployments in a specific namespace
kubectl get deployments -n data-app
```

---

## 🌐 Using Frontend in Browser (CORS / Networking Note)

> When using the frontend in the browser, the frontend code runs in your browser. Hence, it is unable to directly access the backend through service names like `http://backend:5000`.

Use `kubectl exec` to simulate frontend-to-backend communication **from within the cluster**:

```bash
kubectl exec -it frontend-58cbfd96dd-kgh5j -n data-app -- curl -X POST \
  http://backend:5000/submit \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "address": "123 Main St"}'

kubectl exec -it frontend-58cbfd96dd-kgh5j -n data-app -- curl http://backend:5000/users
```

---

## 🍎 Accessing Frontend on macOS with Minikube

### ⚠️ NodePort Limitations on macOS

On macOS, Minikube runs inside a VM or container (depending on the driver). This introduces an additional network layer, which causes **NodePort services to not be directly accessible from the host**.

> Unlike Linux, macOS doesn't expose Minikube NodePorts to the host machine’s interface by default. Instead, NodePorts are available only within the VM/container’s network.
