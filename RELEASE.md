# Release Instructions

## How to Create a Release

1. **Tag the version:**
   ```bash
   git tag v0.2.1
   git push origin v0.2.1
   ```

2. **Automatic build:**
   - GitHub Actions automatically builds and pushes Docker image with tags:
     - `lironyo98/flask_quakeqatch:0.2.1`
     - `lironyo98/flask_quakeqatch:0.2`
     - `lironyo98/flask_quakeqatch:0`
     - `lironyo98/flask_quakeqatch:latest`

3. **Helm chart update:**
   - After successful build, Helm chart repo is automatically updated

## Version Format

Use semantic versioning: `v{MAJOR}.{MINOR}.{PATCH}`

Examples:
- `v0.2.1` - patch release
- `v0.3.0` - minor release  
- `v1.0.0` - major release

## Current Version

Current version: **0.2.0**
