# QuakeWatch - DevOps Pipeline

Flask-based web application for real-time earthquake data visualization. Production-ready with automated CI/CD, version management, and multi-repository deployment strategy.

**📦 Version:** 0.2.0  
**🐳 Docker Image:** `lironyo98/flask_quakeqatch`  
**⎈ Helm Chart:** `lironyo98/flask_quakeqatch-chart`  (separate repo, auto-updated)

---

## 🔄 CI/CD Architecture
1. **🔍 Code Quality** - Lint workflow validates Python code across 3.11-3.13 on every push/PR
2. **🏷️ Version Tagging** - Developer pushes git tag to trigger release
3. **📝 File Updates** - Build workflow updates 6 files with new version and commits to main
4. **🐳 Docker Build** - Image built and pushed to DockerHub with version tag
5. **⎈ Helm Sync** - Separate chart repository (ironyo98/flask_quakeqatch-chart) auto-updated with new image tag and push

### Automated Workflows

**🔍 Lint** → Code quality checks across Python 3.11-3.13 on every push/PR.

**🏗️ Build & Push** → Triggered on version tag push (e.g., `v0.2.1`):
1. Updates 6 version-tracked files in main repo
2. Builds and pushes Docker image to DockerHub
3. Commits changes back to main branch

**⎈ Helm Update** → Triggered after successful build:
1. Reads version from `.bumpversion.cfg`
2. Updates `values-prod.yaml` in separate helm chart repository
3. Commits and pushes to helm repo

### 🔗 Version Synchronization

Release tags automatically update:

**Main Repository:**
- `.bumpversion.cfg` - version source of truth
- `docker-compose.yml`, `pod.yaml`, `k8s/deployment.yaml` - image references
- `README.md`, `Install-README.md` - documentation

**Helm Chart Repository:**
- `values-prod.yaml` - production image tag

---

## 🚀 Release Process

```bash
git tag v0.2.1
git push origin v0.2.1

# Pipeline automatically:
# • Updates all version references
# • Builds Docker image: lironyo98/flask_quakeqatch:0.2.1
# • Updates Helm chart in separate repo
```

---

## 📦 Deployment

### ⎈ Production (Helm)

```bash
helm repo add quakewatch https://lironyo98.github.io/flask_quakeqatch-chart
helm install quakewatch quakewatch/quakewatch
```

### 🐳 Docker

```bash
docker pull lironyo98/flask_quakeqatch:0.2.0
docker run -p 5000:5000 lironyo98/flask_quakeqatch:0.2.0
```

### 🐙 Docker Compose

```bash
docker compose up -d
```

### ☸️ Kubernetes

```bash
kubectl apply -f pod.yaml          # Single pod
kubectl apply -f k8s/              # Full deployment
```

---

## 💻 Local Development

```bash
pip install -r requirements.txt
python app.py
```

Visit http://localhost:5000


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
