# first-k8s-minikube-app

A full-stack web application containerized with Docker and deployed on a local Kubernetes cluster using Minikube. Built as a foundational DevOps project covering containerization, Kubernetes architecture, and service networking.

## Architecture

```
Browser
  │
  ▼
[nginx-service] — NodePort :30090
  │
  ▼
[nginx-deployment] — Reverse Proxy
  │
  ├── / ────────────► Static HTML/JS frontend
  │
  └── /api/ ────────► [api-service] — ClusterIP
                            │
                            ▼
                      [api-deployment] — Express API :8080
```

## Stack

- **Frontend** — Plain HTML/JS, served by Nginx
- **Backend** — Node.js / Express
- **Proxy** — Nginx (reverse proxy + static file server)
- **Containerization** — Docker
- **Orchestration** — Kubernetes (Minikube)

## Project Structure

```
.
├── api/
│   ├── index.js          # Express API — /health and / endpoints
│   ├── package.json
│   └── Dockerfile
├── web/
│   └── index.html        # Frontend — polls /api/health every 5s
├── nginx/
│   ├── nginx.conf        # Reverse proxy config
│   └── Dockerfile
└── k8s/
    ├── namespace.yaml
    ├── configmap.yaml
    ├── api-deployment.yaml
    ├── api-service.yaml
    ├── nginx-deployment.yaml
    ├── nginx-service.yaml
    └── nginx-pod.yaml
```

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Running Locally

**1. Start Minikube**
```bash
minikube start
```

**2. Point Docker to Minikube's daemon**
```bash
eval $(minikube docker-env)
```

**3. Build images**
```bash
docker build -t k8s-app/api:latest ./api
docker build -t k8s-app/nginx:latest -f nginx/Dockerfile .
```

**4. Apply Kubernetes manifests**
```bash
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/api-deployment.yaml
kubectl apply -f k8s/api-service.yaml
kubectl apply -f k8s/nginx-deployment.yaml
kubectl apply -f k8s/nginx-service.yaml
```

**5. Open the app**
```bash
minikube service nginx-service -n k8s-app
```

## Kubernetes Resources

| Resource | Type | Purpose |
|---|---|---|
| `namespace` | Namespace | Isolates all resources under `k8s-app` |
| `configmap` | ConfigMap | Shared config — ports, env vars, nginx.conf |
| `api-deployment` | Deployment | Runs 2 replicas of the Express API |
| `api-service` | ClusterIP | Internal service — Nginx routes to this |
| `nginx-deployment` | Deployment | Runs 2 replicas of Nginx |
| `nginx-service` | NodePort | Exposes Nginx externally on port 30090 |
