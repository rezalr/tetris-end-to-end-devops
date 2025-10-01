## 🚀 End-to-End DevSecOps Kubernetes Project

![Infrastructure Diagram](assets/Infra.gif)

An end-to-end DevSecOps learning project that deploys a Tetris game to AWS EKS using Terraform, builds and ships images with CI/CD, and applies security tooling across the pipeline.

### Repository Structure

- **Infra**: Terraform for AWS networking, IAM, and EKS cluster/node groups.
- **Jenkins-Infra**: Terraform to provision a Jenkins EC2 instance with required IAM and tools.
- **Jenkins-Pipeline-Code**: Jenkinsfiles for building, scanning, and deploying Tetris.
- **Manifest-file**: Kubernetes Deployment and Service for the Tetris app.
- **Tetris-V1**: React Tetris implementation (version 1) with Dockerfile.
- **Tetris-V2**: React Tetris implementation (version 2) with Dockerfile.

### Prerequisites

- AWS account and credentials configured (e.g., `aws configure`)
- Terraform ≥ 1.4, kubectl, Docker
- Optional: Jenkins server (use `Jenkins-Infra`) and a container registry (e.g., ECR/Docker Hub)

### Quick Start

1) Clone
```bash
git clone https://github.com/rezalr/End-to-End-Kubernetes-DevSecOps-Tetris-Project.git
cd End-to-End-Kubernetes-DevSecOps-Tetris-Project
```

2) Run locally (choose V1 or V2)
```bash
cd Tetris-V1   # or Tetris-V2
npm install
npm start
```

3) Build Docker image
```bash
cd Tetris-V1   # or Tetris-V2
docker build -t tetris:local .
```

4) Deploy to Kubernetes (existing cluster)
```bash
kubectl apply -f Manifest-file/deployment-service.yml
kubectl get pods,svc
```

### Provision AWS EKS with Terraform

1) Initialize and plan
```bash
cd Infra
terraform init
terraform plan -var-file=variables.tfvars
```

2) Apply
```bash
terraform apply -var-file=variables.tfvars -auto-approve
```

3) Update kubeconfig
```bash
aws eks update-kubeconfig --name <your-eks-cluster-name> --region <your-region>
```

Then deploy the manifests from `Manifest-file` as shown above.

### Provision Jenkins with Terraform (optional)

1) Create Jenkins EC2
```bash
cd Jenkins-Infra
terraform init
terraform apply -var-file=variables.tfvars -auto-approve
```

2) Use the public IP to access Jenkins, install recommended plugins, and configure credentials (AWS, registry, GitHub). Refer to `Jenkins-Pipeline-Code` for `Jenkinsfile-*` examples:
- `Jenkinsfile-EKS-Terraform`: Infra pipeline
- `Jenkinsfile-TetrisV1`: Build/scan/deploy Tetris V1
- `Jenkinsfile-TetrisV2`: Build/scan/deploy Tetris V2

### Security Tooling

- Trivy for container image scanning
- OWASP Dependency-Check for dependency vulnerabilities
- SonarQube for code quality and security hotspots

Integrate these in Jenkins or run locally as needed.

### Learn More

Follow the detailed guide in this blog: [DevSecOps Mastery — Deploying Tetris on AWS EKS with Jenkins and ArgoCD](https://amanpathakdevops.medium.com/devsecops-mastery-a-step-by-step-guide-to-deploying-tetris-on-aws-eks-with-jenkins-and-argocd-3adcf21b3120)

### License

This project is licensed under the Apache-2.0 license. See the official [LICENSE](http://www.apache.org/licenses/) for details.
