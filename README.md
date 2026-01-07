# ArgoCD Guestbook Project

This repository contains my **GitOps-based deployment of the Guestbook application** using **Argo CD**, **OpenShift**, and **GitHub Actions** as part of my CI/CD course final project.

The goal of this project is to demonstrate a complete GitOps workflow:

- **CI** — Build and publish container images for backend and frontend using GitHub Actions  
- **CD** — Deploy and manage the application state using Argo CD  
- **Isolation** — All resources are uniquely prefixed (`martin-*`) to avoid conflicts in a shared namespace

---

## Repository Structure

.github/
└─ workflows/ # CI pipelines for building & pushing images
backend/ # Go backend + Containerfile
frontend/ # Nginx frontend + Containerfile
k8s/ # Kubernetes manifests managed by Argo CD
README.md


- The **`k8s/`** folder contains all manifests synchronized by Argo CD  
- Backend & frontend images are built and pushed via GitHub Actions

---

##  How the System Works

###  Continuous Integration (GitHub Actions)**

The workflow builds and publishes container images for:

- Backend API (Go)
- Frontend (Nginx)

The workflow only triggers when relevant files change:

- `backend/**`
- `frontend/**`
- `.github/workflows/**`

This avoids unnecessary builds and keeps the pipeline efficient.

---

###  Continuous Deployment (Argo CD)**

Argo CD watches the `k8s/` directory and ensures OpenShift always matches the repo’s desired state.

Managed components include:

- Deployments (backend, frontend, Redis, PostgreSQL)
- Services & routes
- PersistentVolumeClaims
- Resource limits & quotas

Argo CD automatically detects drift and re-applies the configuration.

---

## Namespace & Naming Convention

Because the namespace is shared with classmates, all resources are prefixed with:

martin-guestbook


This ensures:

- No conflicts with other deployments  
- Unique DNS & service names  
- Fair sharing under ResourceQuota rules  

Examples:

- `martin-backend`
- `martin-frontend`
- `martin-redis`
- `martin-postgres`

---

## Deployment (Argo CD)

1. Add a new Argo CD Application pointing to this repo
2. Set the path to:

k8s/


3. Sync the application
4. Retrieve the route:

```bash
