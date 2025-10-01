# Kubernetes Testing Application

A simple Node.js application with Kubernetes deployment and GitHub Actions CI/CD pipeline.

## 🚀 Features

- Simple Express.js REST API
- Docker containerization
- Kubernetes deployment manifests
- GitHub Actions CI/CD pipeline
- Health checks and monitoring
- Auto-scaling ready

## 📁 Project Structure

```
.
├── .github/
│   └── workflows/
│       └── kubernetes-deploy.yml    # CI/CD pipeline
├── k8s/
│   ├── configmap.yaml              # Configuration
│   ├── deployment.yaml             # Kubernetes deployment
│   └── service.yaml                # Kubernetes service
├── .dockerignore                   # Docker ignore file
├── Dockerfile                      # Container definition
├── package.json                    # Node.js dependencies
├── server.js                       # Application code
└── README.md                       # This file
```

## 🛠️ Local Development

### Prerequisites

- Node.js 18+
- Docker
- kubectl (for Kubernetes deployment)

### Running Locally

1. Install dependencies:
   ```bash
   npm install
   ```

2. Start the application:
   ```bash
   npm start
   ```

3. Test the endpoints:
   ```bash
   curl http://localhost:3000/
   curl http://localhost:3000/health
   ```

### Building Docker Image

```bash
# Build the image
docker build -t kubernetes-testing-app .

# Run the container
docker run -p 3000:3000 kubernetes-testing-app

# Test the containerized app
curl http://localhost:3000/
```

## ☸️ Kubernetes Deployment

### Manual Deployment

1. Apply the Kubernetes manifests:
   ```bash
   kubectl apply -f k8s/configmap.yaml
   kubectl apply -f k8s/deployment.yaml
   kubectl apply -f k8s/service.yaml
   ```

2. Check the deployment status:
   ```bash
   kubectl get pods
   kubectl get services
   ```

3. Get the external IP (for LoadBalancer service):
   ```bash
   kubectl get service kubernetes-testing-service
   ```

### Scaling

```bash
# Scale to 5 replicas
kubectl scale deployment kubernetes-testing-app --replicas=5

# Check status
kubectl get pods
```

## 🔄 CI/CD Pipeline

The GitHub Actions workflow automatically:

1. **On every push/PR**: Builds and tests the application
2. **On push to main**: Deploys to Kubernetes cluster

### Setup Requirements

To enable automatic deployment, add these secrets to your GitHub repository:

1. **KUBE_CONFIG**: Base64 encoded Kubernetes config file
   ```bash
   cat ~/.kube/config | base64
   ```

### Workflow Triggers

- **Build**: Triggered on push to `main`/`develop` branches and PRs to `main`
- **Deploy**: Only triggered on push to `main` branch

## 📊 Monitoring & Health Checks

The application includes:

- **Health endpoint**: `GET /health` returns service status
- **Kubernetes liveness probe**: Checks `/health` every 10 seconds
- **Kubernetes readiness probe**: Checks `/health` every 5 seconds
- **Docker health check**: Built into the container

## 🔧 Configuration

Environment variables can be configured via:

1. **ConfigMap** (`k8s/configmap.yaml`):
   - NODE_ENV
   - LOG_LEVEL

2. **Deployment environment** (`k8s/deployment.yaml`):
   - PORT (default: 3000)

## 🌐 API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Main endpoint with app info |
| `/health` | GET | Health check endpoint |

### Response Examples

**GET /**
```json
{
  "message": "Hello from Kubernetes!",
  "version": "1.0.0",
  "timestamp": "2025-10-01T12:00:00.000Z",
  "hostname": "kubernetes-testing-app-xxx-yyy"
}
```

**GET /health**
```json
{
  "status": "healthy"
}
```

## 🚦 Getting Started

1. **Fork this repository**
2. **Clone your fork**:
   ```bash
   git clone https://github.com/YOUR_USERNAME/kubernetes_testing.git
   cd kubernetes_testing
   ```

3. **Set up your Kubernetes cluster** (choose one):
   - Local: minikube, kind, or Docker Desktop
   - Cloud: GKE, EKS, or AKS

4. **Configure GitHub secrets** (for automatic deployment):
   - Add `KUBE_CONFIG` secret with your cluster config

5. **Push to main branch** to trigger deployment

## 🔍 Troubleshooting

### Common Issues

1. **Pod not starting**:
   ```bash
   kubectl describe pod <pod-name>
   kubectl logs <pod-name>
   ```

2. **Service not accessible**:
   ```bash
   kubectl get endpoints
   kubectl describe service kubernetes-testing-service
   ```

3. **Image pull errors**:
   - Ensure the image exists in the registry
   - Check image pull secrets if using private registry

### Useful Commands

```bash
# Get all resources
kubectl get all

# Watch pod status
kubectl get pods -w

# Port forward for local testing
kubectl port-forward service/kubernetes-testing-service 8080:80

# View logs
kubectl logs -f deployment/kubernetes-testing-app
```

## 📝 License

MIT License - feel free to use this as a template for your own projects!

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test locally and with Kubernetes
5. Submit a pull request

---

**Happy Kubernetes-ing! 🎉**