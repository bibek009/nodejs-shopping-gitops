# NodeJS Shopping Application - GitOps CI/CD on Kubernetes

# URLS:
- Repo1, Sample Forked Project Link: https://github.com/bibek009/nodejs-shopping.git 
- Repo2, GitOps config Link: https://github.com/bibek009/nodejs-shopping-gitops.git
- Jenkins CI Server Link: https://jenkins.bibekbajagain.com.np
- ArgoCD CD Server Link: https://argocd.bibekbajagain.com.np
- K8s Cluster: Behind Reverse Proxy in private IPs.
- Deployed App Public link (k8s service => MetalLB Allocated IP => NGINX Reverse Proxy) : https://shopping.bibekbajagain.com.np

All the server resides privately, required ports exposed publicly through reverse proxy.

 
## Project Overview

This project demonstrates a production-inspired GitOps deployment
pipeline for a Node.js shopping application using Kubernetes. It
showcases modern DevOps practices including Continuous Integration,
Continuous Deployment, Infrastructure as Code, containerization, GitOps,
Kubernetes orchestration, and automated deployments.

The application source code and Kubernetes manifests are maintained in
separate repositories following the GitOps principle where Git acts as
the single source of truth.

## Architecture

``` text
Developer
    │
    ▼
GitHub (Application Repository)
    │
    ▼
GitHub Webhook
    │
    ▼
Jenkins CI
 ├── Checkout Source
 ├── Build Docker Image
 ├── Push Image to Docker Hub
 ├── Checkout GitOps Repository
 ├── Update Deployment Image Tag
 └── Push GitOps Repository
    │
    ▼
GitHub (GitOps Repository)
    │
    ▼
ArgoCD
    │
    ▼
Automatic Synchronization
    │
    ▼
Kubernetes Cluster
(1 Master + 2 Workers)
    │
    ▼
MetalLB LoadBalancer
    │
    ▼
Host NGINX Reverse Proxy
    │
    ▼
End Users
```

## Technology Stack

-   GitHub
-   Jenkins
-   Docker
-   Docker Hub
-   Kubernetes
-   ArgoCD
-   GitOps
-   MetalLB
-   Nginx Reverse Proxy
-   MongoDB Atlas
-   ConfigMaps & Secrets
-   rsync + systemd timer
-   Ubuntu Server

## Infrastructure

-   Kubernetes Cluster
    -   1 Master Node
    -   2 Worker Nodes
-   Jenkins on a separate server
-   Nginx reverse proxy for Jenkins, ArgoCD and the application
-   MetalLB for external LoadBalancer services
-   MongoDB Atlas
-   Cloudflare

## Repository Structure

### Application Repository

``` text
nodejs-shopping/
├── app.js
├── package.json
├── views/
├── public/
└── devops/
    ├── docker/
    │   └── Dockerfile
    ├── jenkins/
       └── Jenkinsfile  
```

### GitOps Repository

``` text
nodejs-shopping-gitops/
└── shopping/
    ├── namespace.yaml
    ├── deployment.yaml
    ├── service.yaml
    ├── configmap.yaml
    └── secret.yaml (created in cluster from commandline to expose secrets temporarily. Next plan: to use vaults)
    └── pv.yaml
    └── pvc.yaml
└── argocd/
    ├── application.yaml
```

## CI/CD Workflow

1.  Developer pushes source code to GitHub.
2.  GitHub webhook triggers Jenkins.
3.  Jenkins:
    -   Builds Docker image.
    -   Pushes image to Docker Hub.
    -   Updates deployment manifest in GitOps repository.
    -   Commits and pushes the updated image tag.
4.  ArgoCD detects GitOps repository changes.
5.  ArgoCD synchronizes the Kubernetes cluster automatically.
6.  Kubernetes performs a rolling update.

## Kubernetes Features

-   Deployments
-   Services
-   Namespaces
-   ConfigMaps
-   Secrets
-   Readiness Probes
-   Liveness Probes
-   Rolling Updates
-   Self-Healing (ArgoCD)

## Configuration Management

### ConfigMap

-   MongoDB Host
-   Database Name
-   Application Port

### Secret

-   MongoDB Username
-   MongoDB Password

## Networking

-   MetalLB LoadBalancer
-   Nginx Reverse Proxy
-   Kubernetes Services

## Persistent Upload Storage

Application uploads are stored under hosts as:

``` text
/data/shopping/images
```

Because the lab cluster does not use a distributed storage solution
(such as CephFS, Longhorn or NFS), uploaded images are synchronized
between the two worker nodes using:

-   rsync
-   systemd timer (every 5 seconds)

This provides near real-time synchronization for demonstration purposes.
In production this would typically be replaced by a CSI-backed shared
storage solution.

## Security

-   Jenkins Credentials for GitHub and Docker Hub
-   Kubernetes Secrets
-   HTTPS via Nginx reverse proxy
-   GitOps workflow with Git as the source of truth

## Skills Demonstrated

-   GitOps
-   Kubernetes Administration
-   Jenkins Pipelines
-   Docker
-   ArgoCD
-   GitHhookub Webs
-   MetalLB
-   Reverse Proxy
-   ConfigMaps & Secrets
-   Rolling Deployments
-   Health Checks
-   Linux Administration
-   rsync Automation
-   systemd Timers
-   MongoDB Atlas

## Future Improvements

-   Kustomize or Helm
-   CSI Storage (Longhorn/CephFS/NFS)
-   Horizontal Pod Autoscaler
-   Prometheus & Grafana
-   Centralized Logging
-   Trivy Image Scanning
-   Cert-Manager
-   External Secrets

## Conclusion

This project demonstrates a complete GitOps-based CI/CD pipeline from
source code commit to automated deployment on Kubernetes, highlighting
practical DevOps skills and production-oriented architecture while
remaining suitable for a homelab environment.
