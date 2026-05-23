# mi-app-tf

<img src="https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" alt="GitHub Actions"/>
<img src="https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white" alt="Terraform"/>
<img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker"/>
<img src="https://img.shields.io/badge/Amazon_AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS"/>

Automatic deployment of a Node.js app on AWS using GitHub Actions + Terraform + Docker.

Each manual trigger provisions the infrastructure on AWS, clones the repo on the EC2 instance and starts the app inside a Docker container.

---

## Stack

| Tool | Purpose |
|---|---|
| GitHub Actions | CI/CD pipeline |
| Terraform | Infrastructure as code (EC2 + SG + S3) |
| Docker | Application container |
| AWS EC2 | Server running the app |
| AWS S3 | Terraform state storage |

---

## Repository structure

```
mi-app-tf/
├── .github/
│   └── workflows/
│       ├── deploy.yml          # v1: basic deploy
│       ├── deploy2.yml         # v2: deploy + destroy with state in S3
│       ├── deploy3.yml         # v3: deploy + destroy with composite action
│       └── destroy.yml         # standalone destroy workflow
├── action.yml                  # reusable composite action (v3)
├── app/
│   └── server.js               # Node.js Express app
├── Dockerfile                  # Docker image
├── main.tf                     # Terraform infrastructure
├── package.json
├── package-lock.json
├── .gitignore
└── .terraform.lock.hcl
```

---

## Required GitHub Secrets

Go to **Settings → Secrets and variables → Actions** and add:

| Secret | Description |
|---|---|
| `AWS_ACCESS_KEY_ID` | AWS Academy credential |
| `AWS_SECRET_ACCESS_KEY` | AWS Academy credential |
| `AWS_SESSION_TOKEN` | AWS Academy session token |
| `BUCKET` | S3 bucket name for Terraform state |

---

## Quick start

### Deploy (v3)
1. Go to **Actions → Terraform v3 → Run workflow**
2. Select `Terraform_apply`
3. Access the app at `http://<PUBLIC_IP>`

### Destroy
1. Go to **Actions → Terraform v3 → Run workflow**
2. Select `Terraform_destroy`

> ⚠️ AWS Academy credentials expire every few hours. Update the secrets before each run.

---

## Pipeline versions

- **v1** `deploy.yml` — basic deploy, no state management
- **v2** `deploy2.yml` — deploy + destroy, state stored in S3
- **v3** `deploy3.yml` — same as v2 but uses a composite action to reduce code duplication

---

## App

Root endpoint returns:
```json
{ "response": "Hi, Amazon Web Service" }
```