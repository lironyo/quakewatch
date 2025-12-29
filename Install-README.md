# QuakeWatch

Flask-based web application for real-time and historical earthquake data visualization from USGS API.

**Version:** 0.2.0  
**Docker image:** `lironyo98/flask_quakeqatch`  
**Helm Chart:** `lironyo98/flask_quakeqatch-chart`

## CI/CD Pipeline

Automatic version bump and Docker push on every commit to `main`:

1. **lint.yml** - Python linting (3.11, 3.12, 3.13)
2. **build.yml** - Version bump + Docker build & push
3. **helm-update.yml** - Update Helm chart repo


## Kubernetes Deployment

```bash
helm repo add quakewatch https://github.com/lironyo98/flask_quakeqatch-chart
helm install quakewatch quakewatch/flask_quakeqatch -f values-prod.yaml -n default
```

## Project Structure

```
QuakeWatch/
├── .github/workflows/
│   ├── lint.yml
│   ├── build.yml
│   └── helm-update.yml
├── app.py
├── dashboard.py
├── utils.py
├── k8s/
├── templates/
├── static/
├── Dockerfile
├── docker-compose.yml
├── .bumpversion.cfg
├── version.txt
├── requirements.txt
└── README.md
```
## Quick Start

### Local

```bash
git clone https://github.com/lironyo98/QuakeWatch.git
cd QuakeWatch
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

Visit [http://localhost:5000](http://localhost:5000)

### Docker

```bash
docker compose up -d
```

### Docker Hub

```bash
docker pull lironyo98/flask_quakeqatch:latest
docker run -p 5000:5000 lironyo98/flask_quakeqatch:latest
```

### Kubernetes

```bash
# Add Helm repo
helm repo add quakewatch https://github.com/lironyo98/flask_quakeqatch-chart
helm repo update

# Deploy
helm install quakewatch quakewatch/flask_quakeqatch -f values-prod.yaml -n default

# Access
kubectl port-forward svc/flask_quakeqatch 5000:5000
```