# tracker-iac
The tracker-iac (Infrastructure as Code) project will manage the provisioning of AWS cloud infrastructure using Terraform, ensuring a scalable, secure, and automated environment for the Job Tracker Application Platform.
This project serves as the foundation for all other repositories in the system, including the Backend API and Frontend UI, by defining and managing their infrastructure needs.

---

### Key Objectives:
-   __Standardized Deployment:__ Use Terraform to define infrastructure for __all environments__ (Development, Testing, and Production).
-   __Multi-Repository Support:__ This repository will manage infrastructure for __multiple codebases__ (tracker-API, tracker-UI, any other tracker related repository).
-   __Single AWS Account, Multi-Environment Design:__  
    -   Due to the __lack of multiple AWS accounts__, all environments will be managed within the __same AWS account__.
    -   Resources will be logically separated using __prefixes, IAM policies, and environment-based configurations__.
    -   The infrastructure will be __designed as if it were deployed across multiple accounts__, ensuring future scalability and migration readiness.
-   __Security & Compliance:__  
    -   __OIDC Role-Based Access:__ Secure Terraform operations by using __GitHub Actions with OIDC roles__ to interact with AWS.
    -   __AWS Secrets Manager:__ Securely manage database credentials, API keys, and other sensitive configurations.
    -   __IAM Role Separation:__ Implement least privilege access control for different environments and services.
-   __State Management & Locking:__  
    -   __Terraform state files__ are be stored in __AWS S3__, ensuring centralized and consistent state management.
    -   __State locking__ is enforced using __DynamoDB__ to prevent concurrent infrastructure changes.
-   __Infrastructure Components Managed:__  
    -   __Networking:__   
        VPC, Subnets, Security Groups, Route Tables, and NAT Gateways.
    -   __Compute:__  
        AWS ECS (Fargate) for API and UI workloads.
    -   __Storage:__  
        AWS RDS (PostgreSQL) for database storage, AWS S3 for file storage (resumes).
    -   __Security & Secrets Management:__  
        AWS Secrets Manager, IAM roles & policies.
    -   __Event-Driven Processing:__  
        AWS SQS for job application notifications, AWS SNS for email/SMS notifications.
    -   __Observability & Monitoring:__   
        AWS CloudWatch and Datadog for performance and security monitoring.

---

### Initial Structure Proposal
```python
📦 tracker-iac                      # Root directory of the Terraform repo
├── 📂 modules                      # Reusable Terraform modules for different components
│   ├── 📂 networking               # VPC, Subnets, Security Groups
│   ├── 📂 compute                  # ECS, Lambda, Auto Scaling
│   ├── 📂 database                 # RDS, Parameter Groups
│   ├── 📂 storage                  # S3 Buckets for Resumes and State Files
│   ├── 📂 security                 # IAM, Secrets Manager, Cognito
│   ├── 📂 messaging                # SQS, SNS for notifications
│   ├── 📂 monitoring               # CloudWatch, Datadog integration
│   ├── 📂 ci-cd                    # OIDC IAM Role, GitHub Actions permissions
│   ├── 📂 global                   # Common resources shared across environments
│   └── README.md                   # Description of module usage
│   
├── 📂 environments                 # Environment-specific configurations
│   ├── 📂 dev                      # Development environment
│   │   ├── main.tf                 # Terraform configuration for dev
│   │   ├── variables.tf            # Input variables
│   │   ├── outputs.tf              # Output variables
│   │   ├── terraform.tfvars        # Environment-specific values
│   │   ├── backend.tf              # Remote backend (S3 + DynamoDB)
│   │   ├── providers.tf            # Provider configurations
│   │   └── README.md               # Documentation for dev environment
│   │
│   ├── 📂 tst                      # Testing/Staging environment
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   ├── terraform.tfvars
│   │   ├── backend.tf
│   │   ├── providers.tf
│   │   └── README.md
│   │
│   ├── 📂 prod                     # Production environment
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   ├── terraform.tfvars
│   │   ├── backend.tf
│   │   ├── providers.tf
│   │   └── README.md
│   │
│   └── README.md                   # Overview of environments
│
├── 📂 .github                      # CI/CD scripts for Terraform
|   ├── 📂 actions                  # Action scripts for Terraform
|   |   ├── plan                    # Performs the Terraform Plan Action
|   |   └── apply                   # Performs the Terraform Apply Action
|   └── 📂 workflowa                # Action scripts for Terraform
│       ├── 📜 terraform-plan.yml   # GitHub Actions workflow for planning changes
│       ├── 📜 terraform-apply.yml  # GitHub Actions workflow for applying changes
│       ├── 📜 lint.yml             # Terraform format & validation checks
│       └── README.md               # CI/CD pipeline documentation
│   
├── .gitignore                      # Ignore Terraform state files, logs, and sensitive files
├── .terraform.lock.hcl             # Terraform dependency lock file
├── README.md                       # Repository documentation and usage guide
├── versions.tf                     # Terraform required versions and providers
├── Makefile                        # Common Terraform commands for easy execution
└── LICENSE                         # Repository license file
```

---
