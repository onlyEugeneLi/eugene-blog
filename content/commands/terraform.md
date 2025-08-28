**A simple Terraform Workflow**
1. Write configuration file (resource block is essential)
1. Run command `terraform init`
1. Run command `terraform plan`
1. Run command `terraform apply`

**Structure** \
Provider -- Resoruce_type -- Arguments 

**Classic Terraform Configuration Lifecycle**
```bash
terraform init # Initialise terraform

terraform validate # Validate syntax

terraform plan # Preview resources that will be provisioned

terraform apply -auto-approve # Provision resources

terraform show # Inspects the state file and displays the resource details

terraform destroy # Delete resources provisioned
```
