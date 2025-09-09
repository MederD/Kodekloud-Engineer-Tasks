The Nautilus DevOps team is adopting strict naming conventions for all IAM resources using Terraform. They’ve asked for help enforcing lowercase, hyphenated names based on inputs like project and team.  
Your task as a DevOps engineer is to complete the following using Terraform:  
    Create an IAM User The user name must be derived using the format project-team-user, all lowercase, and non-alphanumeric characters (except dashes) replaced with -.  
    Create an IAM Role Use the same naming logic for the role name, ending in -role, and attach an assume role policy for EC2.  
    Tagging: Both resources must be tagged with:  
        Project: devops  
        Team: dev-team  
        ManagedBy: Terraform  
        Env: dev  
    Additionally, the IAM role should have:  
        RoleType: EC2  
    Use locals block within main.tf to:  
        Derive sanitized project/team names  
        Create the resource name prefix  
        Define reusable common tags  
    Create the main.tf file (do not create a separate .tf file) to provision the IAM Role & User as per the required values.  
    Use variables.tffile with the following:  
        KKE_PROJECT: name of the project(must be non-empty).  
        KKE_TEAM: name of the team (only letters, digits, dashes or underscores)  
        KKE_ENVIRONMENT: name of the environment  
    Use terraform.tfvarsfile to input the values.  
    Use outputs.tffile to output the following:  
        kke_user_name: name of the created user.  
        kke_role_name: name of the created role.  
        kke_tags_applied: tags applied to the IAM User.  

Solution:  
`main.tf:`  
```terraform
locals {
  project = lower(replace(replace(var.KKE_PROJECT, "[^a-zA-Z0-9_-]", "-"), "_", "-"))
  team    = lower(replace(replace(var.KKE_TEAM, "[^a-zA-Z0-9_-]", "-"), "_", "-"))
  name_prefix  = "${local.project}-${local.team}"

  common_tags = {
    Project   = local.project
    Team      = local.team
    ManagedBy = "Terraform"
    Env       = var.KKE_ENVIRONMENT
  }

  role_tags = merge(local.common_tags, { RoleType = "EC2" })
}

resource "aws_iam_user" "kke_user" {
  name = "${local.name_prefix}-user"
  tags = local.common_tags
}

resource "aws_iam_role" "kke_role" {
  name = "${local.name_prefix}-role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = { Service = "ec2.amazonaws.com" }
        Action = "sts:AssumeRole"
      }
    ]
  })
  tags = local.role_tags
}
```
[reference](https://developer.hashicorp.com/terraform/language/functions/replace)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
