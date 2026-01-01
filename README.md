# Ride Sharing Platform - Infrastructure Repository

This repository contains the complete infrastructure definition for the Ride Sharing Platform.

## 📂 Repository Structure

```text
.
├── charts/                     # Helm Charts
│   └── ride-sharing/           # Main application chart
├── terraform/                  # Infrastructure as Code (AWS/GCP)
│   └── README.md
├── argocd/                     # ArgoCD GitOps Manifests
│   └── README.md
└── QUICKSTART.md               # Quickstart guide
```

## 🚀 Getting Started

### 1. **Deploying with Helm**

The main application chart is located in `charts/ride-sharing`.

```bash
cd charts/ride-sharing

# Deploy to Development
kubectl create namespace ride-sharing-dev
helm upgrade --install ride-sharing . -f values-dev.yaml -f secrets-dev.yaml

# Deploy to Production
kubectl create namespace ride-sharing-prod
helm upgrade --install ride-sharing . -f values-prod.yaml -f secrets-prod.yaml
```

See `QUICKSTART.md` for detailed instructions.

### 2. **Infrastructure (Terraform)**

Coming soon - will define EKS cluster, VPC, and databases.

### 3. **GitOps (ArgoCD)**

Coming soon - will define Application of Applications pattern.
