# Terraform_biznesASH

# BiznesAsh Terraform Deployment

This repository contains a modular Terraform setup to provision a multi-tier AWS architecture for a simple Flask-based mock application. The infrastructure simulates a real-world high-availability design with public access and secure backend communication.

---

## Architecture Overview

- **VPC** with 3 public subnets (multi-AZ)
- **EC2** instances (x2) hosting Flask app (manual SSH deployment)
- **Amazon RDS** PostgreSQL DB (non-public, accessible from EC2 only)
- **Security Groups** to control traffic (SSH, HTTP, PostgreSQL)

---

## Project Structure

```
terraform/
├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── modules/
│   ├── vpc/
│   ├── ec2/
│   ├── rds/
│   └── security/
```

---

## How to Deploy

```bash
git clone https://github.com/<your-username>/biznesash-terraform.git
cd biznesash-terraform
terraform init
terraform validate
terraform plan
terraform apply
```

Ensure `terraform.tfvars` includes:

```hcl
db_password = "yourStrongPassword!"
key_name    = "your-ec2-key"
```

---

## Security

- **EC2 SG**: Allows port `22` (SSH) and `5000` (Flask app).
- **RDS SG**: Allows port `5432` **only** from EC2 SG.
- **RDS** is **not publicly accessible** (secured).

---

## Testing

- Access app in browser: `http://<EC2_PUBLIC_IP>:5000`
- SSH into EC2 and connect to DB with:
  ```bash
  psql -h <RDS_ENDPOINT> -U dbuser -d postgres
  ```

---

## Manual Fixes

- RDS password reset via AWS Console
- RDS SG manually updated to restrict access to EC2 SG
- Avoided `terraform apply` after manual fixes to preserve state

---

## Outcome

- Fully modular, working infrastructure
- Flask app + RDS integration successfully tested
- Used project BiznesAsh 
