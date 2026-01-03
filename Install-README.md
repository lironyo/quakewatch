# QuakeWatch - DevOps Pipeline

Flask-based web application for real-time earthquake data visualization. Production-ready with automated CI/CD, version management, and multi-repository deployment strategy.

**Current Version:** 0.2.0  
**Docker Repository:** `lironyo98/flask_quakeqatch`  
**Helm Chart Repository:** `lironyo/flask_quakeqatch-chart` (separate repo, auto-updated)

---

## CI/CD Pipeline

**lint.yml** - Push to main / PR  
Python quality checks across Python 3.11, 3.12, 3.13. Fails if code quality threshold not met.

**build.yml** - Git tag `v*`  
Extracts version, updates all config files, commits to main, builds and pushes Docker image to Docker Hub.

**helm-update.yml** - Triggered after build success  
Reads version from `.bumpversion.cfg`, updates external Helm chart repository with new image version.

**Strategy:** Main repository triggers automatic updates to separate Helm chart repository.

---

## Deployment

**Docker**
```bash
docker pull lironyo98/flask_quakeqatch:latest
docker run -p 5000:5000 lironyo98/flask_quakeqatch:latest
```

**Docker Compose**
```bash
docker compose up -d
```

**Kubernetes with Helm**
```bash
helm repo add quakewatch https://github.com/lironyo/flask_quakeqatch-chart
helm repo update
helm install quakewatch quakewatch/flask_quakeqatch -f values-prod.yaml -n production
```

---

## Release Process

**Automated flow:**
1. Build workflow triggers on tag
2. Extracts version 
3. Updates all config files
4. Commits to main branch
5. Builds Docker image: `lironyo98/flask_quakeqatch:(with the relavent tag version by github action autumation)
6. Helm workflow triggers
7. Updates Helm chart repository with version 
8. ✅ Complete

---

## Project Structure

```
QuakeWatch/
├── .github/workflows/
│   ├── lint.yml              # Code quality checks
│   ├── build.yml             # Version + Docker build
│   └── helm-update.yml       # Updates Helm chart repo
├── app.py
├── dashboard.py
├── utils.py
├── Dockerfile
├── docker-compose.yml
├── .bumpversion.cfg          # Version source of truth
├── requirements.txt
├── k8s/                      # Kubernetes manifests
├── templates/                # Flask templates
└── static/                   # Assets
```

---

## Local Development

**Direct Flask**
```bash
git clone https://github.com/lironyo98/QuakeWatch.git
cd QuakeWatch
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python app.py
```
Visit http://localhost:5000

**Docker**
```bash
docker build -t quakewatch:local .
docker run -p 5000:5000 quakewatch:local
```

**Docker Compose**
```bash
docker compose up
```

**Kubernetes (Local)**
```bash
kubectl apply -f pod.yaml
kubectl apply -f k8s/
kubectl port-forward svc/flask_quakeqatch 5000:5000
```
Visit http://localhost:5000
