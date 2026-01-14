# QuakeWatch

Flask-based web application for real-time earthquake data visualization. Production-ready with automated CI/CD, version management, and multi-repository deployment strategy.

**📦 Version:** 0.2.1  
**🐳 Docker Image:** `lironyo98/flask_quakeqatch`  
**⎈ Helm Chart:** `lironyo/quakewatch-chart`  (separate repo, auto-updated)

---

## 🔄 CI/CD Pipeline

The project uses a three-workflow CI/CD pipeline that automates everything from code quality to production deployment.

### ⚙️ How It Works

**1. 🔍 Code Quality** (`lint.yml`)
- Runs on every push and PR
- Tests Python code across versions 3.11, 3.12, and 3.13
- Catches syntax errors and style issues before merge

**2. 🐳 Build & Push Docker** (`build.yml`)
- Triggers when you push a version tag (like `v0.2.1`)
- Extracts version from the tag (or falls back to `.bumpversion.cfg`)
- Updates version in 6 files: `.bumpversion.cfg`, `docker-compose.yml`, `pod.yaml`, `k8s/deployment.yaml`, `README.md`, `Install-README.md`
- Commits these changes back to main
- Builds Docker image and pushes to Docker Hub as `lironyo98/flask_quakeqatch:0.2.1`

**3. ⎈ Deploy and Update** (`Deploy and Update.yml`)
- Runs after successful Docker build
- Clones the Helm chart repo (`lironyo/quakewatch-chart`)
- Updates `Chart.yaml` (appVersion) and `values-prod.yaml` (image tag)
- Commits and pushes to chart repo

### 🔗 Version Management

Everything stays in sync through `.bumpversion.cfg` as the source of truth:
- Main repo: All deployment manifests and docs
- Docker Hub: Tagged container images
- Helm chart repo: Production configuration

One tag push updates everything automatically.

---

## 🚀 Release Process

```bash
# Tag the release
git tag v0.2.1
git push origin v0.2.1

# The pipeline handles the rest:
# - Updates version in all files
# - Builds and pushes Docker image to Docker Hub
# - Updates Helm chart repository
# - Takes about 3-5 minutes total
```

Check the release:
```bash
docker pull lironyo98/flask_quakeqatch:0.2.1
# or
helm repo update && helm search repo quakewatch
```

---

## 📦 Deployment

### ⎈ Production (Helm)

```bash
helm repo add quakewatch https://lironyo.github.io/quakewatch-chart
helm install quakewatch quakewatch/quakewatch
```

### 🐳 Docker

```bash
docker pull lironyo98/flask_quakeqatch:0.2.1
docker run -p 5000:5000 lironyo98/flask_quakeqatch:0.2.1
```

### 🐙 Docker Compose

```bash
docker compose up -d
```

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

---

## 📋 Project Structure

```
QuakeWatch/
├── .github/workflows/
│   ├── lint.yml                # Code validation
│   ├── build.yml               # Build and push to Docker Hub
│   └── Deploy and Update.yml   # Sync Helm chart
├── app.py
├── dashboard.py
├── utils.py
├── Dockerfile
├── docker-compose.yml
├── .bumpversion.cfg            # Version source of truth
├── requirements.txt
├── k8s/                        # Kubernetes manifests
├── templates/                  # Flask templates
└── static/                     # Assets
```
