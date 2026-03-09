# 🚀 Kubernetes Lab 3 – Multi-Environment Deployment (Dev & Staging)

This lab demonstrates how to deploy the same application in **two isolated Kubernetes environments** using **Namespaces**.

The goal is to simulate a real-world scenario where a company runs the same application in different environments such as:

* **Development (dev)** – for developers
* **Staging (staging)** – for testing before production

Both environments run the same architecture but remain **fully isolated**.

---

# 🏗 Architecture

The application consists of two components:

### 1️⃣ Backend

A simple **Python API** that provides information about the running pod.

Endpoints include:

```
/info
/health
/
```

The backend also exposes:

* Pod name
* Namespace

---

### 2️⃣ Frontend

A **Nginx reverse proxy** that forwards API requests to the backend service.

Flow:

```
User → Frontend (Nginx) → Backend Service → Backend Pod
```

---

# 🧱 Kubernetes Architecture

Each environment contains:

```
Namespace
 ├── backend-pod
 ├── backend-service
 └── frontend-pod
```

Both namespaces use the same service name:

```
backend-service
```

Kubernetes DNS automatically resolves it **within the same namespace**.

---

# 📦 Project Structure

```
Lab-3
│
├── Dockerfile.backend
├── Dockerfile.frontend
├── backend-app.py
├── nginx.conf
│
├── dev-environment.yaml
├── staging-environment.yaml
│
└── screenshots
```

---

# ⚙️ Step 1 – Build Docker Images

```bash
docker build -f Dockerfile.backend -t backend-app:latest .
docker build -f Dockerfile.frontend -t frontend-app:latest .
```

---

# ☸️ Step 2 – Load Images into Minikube

```bash
minikube image load backend-app:latest
minikube image load frontend-app:latest
```

Verify:

```bash
minikube image ls | grep app
```

---

# 📂 Step 3 – Create Namespaces

```bash
kubectl create namespace dev
kubectl create namespace staging
```

Verify:

```bash
kubectl get namespaces
```

---

# 🚀 Step 4 – Deploy Dev Environment

```bash
kubectl apply -f dev-environment.yaml
```

Verify:

```bash
kubectl get all -n dev
```

---

# 🧪 Step 5 – Deploy Staging Environment

```bash
kubectl apply -f staging-environment.yaml
```

Verify:

```bash
kubectl get all -n staging
```

---

# 🔗 Step 6 – Test Internal Service Communication

Test inside **dev** namespace:

```bash
kubectl exec -n dev frontend-pod -- wget -qO- http://backend-service:8080/info
```

Test inside **staging** namespace:

```bash
kubectl exec -n staging frontend-pod -- wget -qO- http://backend-service:8080/info
```

Each environment should return its own namespace.

---

# 🌐 Step 7 – Access Frontend via Port Forward

Example:

```bash
kubectl port-forward -n dev pod/frontend-pod 8081:80
```

Then open:

```
http://localhost:8081
```

Or test with:

```bash
curl http://localhost:8081
```

---

# 📸 Screenshots

## Dev Environment Running

*Add screenshot here*

```
kubectl get all -n dev
```

---

## Staging Environment Running

*Add screenshot here*

```
kubectl get all -n staging
```

---

## Service Communication Test

*Add screenshot here*

```
kubectl exec -n dev frontend-pod -- wget -qO- http://backend-service:8080/info
```

---

## Port Forward Test

*Add screenshot here*

```
kubectl port-forward -n dev pod/frontend-pod 8081:80
```

---

# 🎯 What This Lab Demonstrates

This lab demonstrates several important Kubernetes concepts:

* Namespaces for environment isolation
* Service discovery using Kubernetes DNS
* Multi-environment deployment
* Internal communication between services
* Accessing pods using port-forward

---

# 🧠 Key Learning Outcome

By completing this lab you learn how to:

* Deploy the same application in multiple environments
* Use **Kubernetes namespaces** for isolation
* Connect services using **internal DNS**
* Expose services locally using **port-forward**

---

# 👨‍💻 Author

Kubernetes DevOps Practice Lab


# 📸 Required Verification Screenshots

The following screenshots are required to verify that both environments are working correctly.

---

# 1️⃣ Dev Environment – Pods & Services

Run the following command to verify that the **pods and services in the `dev` namespace are running**:

```
kubectl get pods,svc -n dev
```

Take a screenshot of the output and place it in:

```
screenshots/pods,svc -n dev.png
```

![Dev Environment](screenshots/pod-dev.png)

---

# 2️⃣ Staging Environment – Pods & Services

Run the following command to verify that the **pods and services in the `staging` namespace are running**:

```
kubectl get pods,svc -n staging
```

Take a screenshot of the output and place it in:

```
screenshots/pods,svc -n staging.png
```

![Dev Environment](screenshots/pods,svc -n staging.png)

---

# 3️⃣ Port Forwarding (Dev Environment)

Run the following command to forward the frontend pod port to your local machine:

```
kubectl port-forward pod/frontend-pod 8082:80 -n dev
```

Take a screenshot showing the port-forward command running.

Save it as:

```
screenshots/portForwarding -n dev.png
```

![Dev Environment](screenshots/portForwarding -n dev.png)

---

# 4️⃣ Access the Application from Browser (Dev)

Open your browser and navigate to:

```
http://localhost:8082
```

Take a screenshot showing the application running in the browser.

Save it as:

```
screenshots/Browser.png
```

![Dev Environment](screenshots/Browser.png)

---

# 5️⃣ Access the Application from Browser (Staging)

Repeat the same process for the **staging environment**.

Take a screenshot of the browser showing the application response.

Save it as:

```
screenshots/Browser...staging.png
```

![Dev Environment](screenshots/Browser...staging.png)
