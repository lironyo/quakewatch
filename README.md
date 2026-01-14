<<<<<<< HEAD
# QuakeWatch - DevOps Pipeline

Flask-based web application for real-time earthquake data visualization. Production-ready with automated CI/CD, version management, and multi-repository deployment strategy.

**Current Version:** 0.2.0  
**Docker Repository:** `lironyo98/flask_quakeqatch`  
**Helm Chart Repository:** `lironyo/flask_quakeqatch-chart` (separate repo, auto-updated)

---

## CI/CD Pipeline

**lint.yml** - Push to main / PR  
Python quality checks across Python 3.11, 3.12, 3.13. Fails if code quality threshold not met.

**build.yml** - Git tag `v*` (e.g., `v0.2.1`)  
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
=======
# QuakeWatch

Flask-based web application for real-time earthquake data visualization. Production-ready with automated CI/CD, version management, and multi-repository deployment strategy.

**📦 Version:** 0.2.0  
**🐳 Docker Image:** `lironyo98/flask_quakeqatch`  
**⎈ Helm Chart:** `lironyo98/flask_quakeqatch-chart`  (separate repo, auto-updated)

---

## 🔄 CI/CD Architecture and Pipeline
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
3. Commits and pushes to helm repo that push the helm to dockerhub with the new tag

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
git tag v0.2.0
git push origin v0.2.0

# Pipeline automatically:
# • Updates all version references
# • Builds Docker image: lironyo98/flask_quakeqatch:0.2.0
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

>>>>>>> origin/main
```bash
docker compose up -d
```

<<<<<<< HEAD
**Kubernetes with Helm**
```bash
helm repo add quakewatch https://github.com/lironyo/flask_quakeqatch-chart
helm repo update
helm install quakewatch quakewatch/flask_quakeqatch -f values-prod.yaml -n production
```

---

=======
### ☸️ Kubernetes

```bash
kubectl apply -f pod.yaml
kubectl apply -f k8s/
kubectl port-forward svc/flask_quakeqatch 5000:5000
```
---

## 💻 Local Development

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



>>>>>>> origin/main
## Release Process

**Automated flow:**
1. Build workflow triggers on tag
2. Extracts version 
3. Updates all config files
4. Commits to main branch
5. Builds Docker image: `lironyo98/flask_quakeqatch:(with the relavent tag version by github action autumation)
6. Helm workflow triggers
<<<<<<< HEAD
7. Updates Helm chart repository with version 0.2.1
=======
7. Updates Helm chart repository with version 
>>>>>>> origin/main
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
<<<<<<< HEAD

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
=======
>>>>>>> origin/main
