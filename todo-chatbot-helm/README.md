# Todo Chatbot Helm Chart

This Helm chart deploys the Todo Chatbot application on Kubernetes.

## Components

- **Frontend**: Next.js application (2 replicas by default)
- **Backend**: FastAPI application
- **Database**: PostgreSQL 15

## Prerequisites

- Kubernetes cluster (e.g., minikube)
- Helm 3.x
- Docker images built and available:
  - `todo-frontend:v1`
  - `todo-backend:v1`

## Installation

### 1. Load Docker images into minikube (if using minikube)

```bash
minikube image load todo-frontend:v1
minikube image load todo-backend:v1
```

### 2. Install the chart

```bash
helm install todo-chatbot ./todo-chatbot-helm
```

### 3. Check deployment status

```bash
kubectl get pods
kubectl get services
```

## Accessing the Application

### Frontend (NodePort)
```bash
minikube service todo-chatbot-frontend
```

Or get the URL:
```bash
minikube service todo-chatbot-frontend --url
```

The frontend will be accessible at the NodePort (default: 30080).

### Backend API
```bash
kubectl port-forward service/todo-chatbot-backend 8000:8000
```

Then access at: http://localhost:8000/docs

## Customization

Edit `values.yaml` to customize:

- Replica counts
- Resource limits
- Environment variables
- Service types
- Image tags

Example: Change frontend replicas:
```yaml
frontend:
  replicaCount: 3
```

## Upgrade

```bash
helm upgrade todo-chatbot ./todo-chatbot-helm
```

## Uninstall

```bash
helm uninstall todo-chatbot
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `frontend.replicaCount` | Number of frontend replicas | `2` |
| `backend.replicaCount` | Number of backend replicas | `1` |
| `database.enabled` | Enable PostgreSQL database | `true` |
| `frontend.service.type` | Frontend service type | `NodePort` |
| `frontend.service.nodePort` | Frontend NodePort | `30080` |
| `backend.service.type` | Backend service type | `ClusterIP` |

## Troubleshooting

### Check pod logs
```bash
kubectl logs -l app.kubernetes.io/component=frontend
kubectl logs -l app.kubernetes.io/component=backend
kubectl logs -l app.kubernetes.io/component=database
```

### Describe pods
```bash
kubectl describe pod <pod-name>
```

### Check services
```bash
kubectl get svc
```
