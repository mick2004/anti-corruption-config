# Anti-Corruption Config Repository

## Overview
This repository stores configuration files for the **Anti-Corruption Layer** and is managed using **GitOps** with **ArgoCD**. Any changes committed to this repository trigger automated updates to the **API, Infrastructure, and Database** within the Kubernetes cluster.

## Repository Structure
```
anti-corruption-config/
├── database/                   # Database configurations
│   ├── postgres.yaml           # PostgreSQL deployment
│   ├── restart-patch.yaml      # Patch for database restart
├── api/                        # API configurations
│   ├── deployment.yaml         # API deployment settings
│   ├── kustomization.yaml      # Kustomize configuration
│   ├── api-config.yaml         # API-specific configurations
│   ├── config.yaml             # Defines API mappings
│   ├── service.yaml            # API service definition
│   ├── rollout-patch.yaml      # Handles API rollout updates
```

## GitOps with ArgoCD
ArgoCD is deployed as part of `argocd.tf` in the **main infrastructure repository** and is responsible for continuously monitoring and applying changes from this repository.

### Configuration Changes
ArgoCD monitors this repository and automatically synchronizes changes:

1. **API Changes** (`api/config.yaml`)
   - Defines API endpoints and mappings.
   - Changes here trigger an **API restart** with the new configuration.

2. **Infrastructure Changes** (`api/deployment.yaml`)
   - Handles API deployment updates.
   - Terraform updates Kubernetes resources when changes are pushed.

3. **Database Changes** (`database/postgres.yaml`)
   - Defines database deployments.
   - Any updates here trigger an automatic database update.

## Workflow
1. **Developer Commits Configuration Updates**
   - Changes to `config.yaml`, `deployment.yaml`, or `postgres.yaml` are committed to `main`.
2. **ArgoCD Detects Changes**
   - Syncs new configurations and applies them to the Kubernetes cluster.
3. **API and Database are Updated**
   - The API restarts with the new configuration.
   - Database updates are applied automatically.

## How to Apply Changes
To update API or database configurations:
```bash
git add .
git commit -m "Update API mapping or DB config"
git push origin main
```
ArgoCD will automatically detect and apply the changes within minutes.

## Summary
- **GitOps-Driven**: Changes are applied automatically via ArgoCD.
- **No Manual Deployments**: Developers only update configurations.
- **Separation of Concerns**: API, infrastructure, and database configs are stored separately.

This repository ensures a smooth, automated workflow for managing the **Anti-Corruption Layer** configuration.
