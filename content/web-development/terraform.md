# Terraform Basics

Reference: \
[Udemy course - Learn the basics of Terraform with real hands on labs right in your browser](https://www.udemy.com/share/1057P2/)

**A simple Terraform Workflow**
1. Write configuration file (resource block is essential)
1. Run command `terraform init`
1. Run command `terraform plan`
1. Run command `terraform apply`

**Resource Block Structure** \
Provider\
Resoruce_type\
Arguments 

```
resource "<provider-name>_<resource-type>" "<custom-resource-name>" {
    argument_1 = "boo"
    argument_2 = "foo"
}
```

**Classic Terraform Configuration Lifecycle**
```bash
terraform init # Initialise terraform

terraform validate # Validate syntax

terraform plan # Preview resources that will be provisioned

terraform apply -auto-approve # Provision resources

terraform show # Inspects the state file and displays the resource details

terraform destroy # Delete resources provisioned
```

## Configuration Directory

root configuration directory

Terraform considers any file with `.tf` extension suffix.

Directory styles
1. File-resource 1-to-1
1. `main.tf`: 1 file contains all resource blocks required to provision the infrastructure
1. 

### File name convention

| File Name | Purpose | 
|-----------|---------|
| main.tf   | main config file containing resource definition |
| variables.tf | Contains variable declarations |
| outputs.tf | Contains outputs from resources | 
| provider.tf | Contains Provider definition |

## Using Input Variables `variables.tf`

Hard coded values -- assigning values to arguments in a resource block in the `main.tf` file 

> Hard coding value is not a good idea. 😿

**In `variables.tf` file:**

```
varibale "filename" {
    default = "root/pets.txt"
}
```

**In `main.tf` / resource definition block:**

```
resource "local_file" "pet" {
    filename = var.filename
}
```

Make update to the resources --> making chagnes to the existing arguments in `variables.tf`

### Variable block

Three arguments
1. default
1. type (optional): string, number, bool, any, list, map (similar to `dict` in Python), object, tuple
1. description

### Declaring variables in command lines

> Leave variable block empty, you'll be prompted to enter values for each variable when running `terraform apply`

<u>Approach 1: Command Line Flags</u>

```bash
terraform apply -var "filename=/root/pets.txt" -var "arugment_other=foo bar"
```

<u>Approach 2: Setting Environment Variables</u>

```bash
export TF_VAR_filename="/root/pets.txt"

terraform apply
```

<u>Approach 3: Variable Definition Files **`terraform.tfvars`**</u>

<span style="color:orange">**Automatically loaded**</span>. variable definition file names: `terraform.tfvars` / `terraform.tfvars.json` / `*.auto.tfvars` / `*.auto.tfvars.json`

In `terraform.tfvars` file:

```
filename = "/root/pets.txt"
``

```bash
terraform apply
```

Precedence order to accept values

Approach 1                                      <span style="color:red">**>**</span> 
Approach 3:`*.auto.tfvars` (alphabetical order) <span style="color:red">**>**</span> 
Approach 3: `terraform.tfvars`                  <span style="color:red">**>**</span> 
Approach 2

## Resource Attribute


### Attribute reference

Interpolation sequence

```bash
${random_pet.my-pet.id}
```

### Resource Dependencies

> Ensures resources are created in the correct order (after other resources are created)

```terraform
resource "aws_instance" "app_server" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"

    tags = {
        Name = "${var.app_region}-AppServerInstance"
    }

    depends_on = [ aws_dynamodb_table.payroll_db,
                   aws_s3_bucket.payroll_data ]
}
```

### Output variables

## Terraform Modules

### What are modules?

Why use modules?

All resource blocks in a single configuration file:
- Complex configuration files
- Duplicate code
- Increase Risk
- Limits reuseability

**Module**: A set of configuration files (`main.tf`, `variables.tf`, ...) in a folder

Development module

```
module "module-name" {
    source = "../path-to-module"
}
```

## Functions and Condition Expressions

### Condition Expressions

` condition ? true_val : false_val`