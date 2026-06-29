# 🔐 End-to-End DevSecOps Pipeline

> A production-grade DevSecOps pipeline integrating automated security scanning, secret management, and policy enforcement at every stage of the software delivery lifecycle — from commit to deployment.

Built from real patterns used across regulated environments including financial services and healthcare, where security gates aren't optional.

---

## 🏗️ Pipeline Architecture

```
┌─────────────┐    ┌──────────────┐    ┌─────────────────┐    ┌────────────────┐
│  Code Push  │───▶│  SAST / SCA  │───▶│ Build & Scan    │───▶│  Deploy to K8s │
│  GitHub     │    │  SonarQube   │    │ Docker + Trivy  │    │  EKS / AKS     │
│             │    │  Checkmarx   │    │ Checkov (IaC)   │    │                │
│             │    │  Snyk        │    │ OWASP ZAP (DAST)│    │                │
└─────────────┘    └──────────────┘    └─────────────────┘    └────────────────┘
       │                  │                    │                       │
       ▼                  ▼                    ▼                       ▼
  OIDC Auth          Quality Gate         Image pushed           Secrets injected
  (keyless)          blocks merge         to ECR/ACR             via Vault / ASM
```

---

## 🔒 Security Stages

| Stage | Tool | What It Catches |
|-------|------|----------------|
| SAST | SonarQube, Checkmarx | Code vulnerabilities, SQL injection, XSS |
| Dependency Scan | Snyk | Vulnerable libraries and transitive deps |
| Container Scan | Trivy, Anchore | CVEs in base images and packages |
| IaC Scan | Checkov | Misconfigured Terraform / CloudFormation |
| DAST | OWASP ZAP | Runtime vulnerabilities in deployed app |
| Secret Detection | git-secrets, Vault | Leaked credentials in code |
| Compliance | AWS Config / Azure Policy | Drift from CIS Benchmarks |

---

## ✅ Key Features

- **OIDC keyless authentication** — GitHub Actions to AWS/Azure, zero long-lived credentials
- **Automated quality gates** — pipelines block on CRITICAL/HIGH findings; nothing reaches production with known vulnerabilities
- **HashiCorp Vault integration** — dynamic secrets injected at runtime, not baked into images
- **Multi-environment promotion** — dev → non-prod → pre-prod → production with manual approval gates
- **Consolidated security reporting** — Python scripts generate unified dashboards from SonarQube, Checkmarx, and Trivy outputs
- **Container image hardening** — non-root users, read-only filesystems, distroless base images
- **Zero-trust networking** — Istio mTLS between all services, NetworkPolicies enforced

---

## 📁 Repository Structure

```
├── .github/
│   └── workflows/
│       ├── sast.yml              # SonarQube + Checkmarx gates
│       ├── container-scan.yml    # Trivy image scanning
│       ├── iac-scan.yml          # Checkov Terraform scanning
│       ├── dast.yml              # OWASP ZAP dynamic scan
│       └── deploy.yml            # EKS/AKS deployment with Vault
├── terraform/
│   ├── modules/                  # Reusable IaC modules
│   └── environments/             # Per-environment configs
├── kubernetes/
│   ├── base/                     # Kustomize base manifests
│   ├── overlays/                 # Environment overlays
│   └── policies/                 # NetworkPolicies, PodSecurityPolicies
├── vault/
│   └── policies/                 # Vault auth policies per service
├── scripts/
│   └── security-report.py        # Consolidated security dashboard
└── docs/
    └── runbooks/                 # Incident response playbooks
```

---

## 🛠️ Tech Stack

![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=flat&logo=terraform&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=github-actions&logoColor=white)
![Azure DevOps](https://img.shields.io/badge/Azure_DevOps-0078D7?style=flat&logo=azure-devops&logoColor=white)
![SonarQube](https://img.shields.io/badge/SonarQube-4E9BCD?style=flat&logo=sonarqube&logoColor=white)
![HashiCorp Vault](https://img.shields.io/badge/Vault-FFEC6E?style=flat&logo=vault&logoColor=black)
![AWS](https://img.shields.io/badge/AWS-FF9900?style=flat&logo=amazon-aws&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat&logo=microsoft-azure&logoColor=white)

---

## 🌍 Real-World Context

These patterns are drawn from production experience across:
- **Lloyds Banking Group** — certificate lifecycle automation, Azure DevOps pipelines in a regulated banking environment
- **GlobalLogic** — EKS security pipelines for a FinTech insurance platform processing 100K+ policies/day
- **OneAdvanced** — Python-based security scanning integration generating consolidated reports with automated quality gates

---

## 📖 Related

- [Mastering_AWS_Terraform](https://github.com/SaiJithendraGonji/Mastering_AWS_Terraform) — IaC modules this pipeline scans and deploys
- [HELM-Charts](https://github.com/SaiJithendraGonji/HELM-Charts) — Helm charts deployed by this pipeline
- [CKA-Preparation](https://github.com/SaiJithendraGonji/CKA-Preparation) — Kubernetes depth behind the deployment stages
