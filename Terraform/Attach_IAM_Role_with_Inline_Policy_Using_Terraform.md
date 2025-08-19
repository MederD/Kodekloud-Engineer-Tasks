The Nautilus DevOps team is setting up IAM-based access control for internal AWS resources. They need to create an IAM Role and an IAM Policy using Terraform and attach the policy to the role.  
    Create an IAM Role named devops-role.  
    Create an IAM Policy named devops-policy that allows listing EC2 instances.  
    Attach the policy to the role  
    Create the main.tf file (do not create a separate .tf file) to provision a Role, policy and attach it.  
    Use the variables.tf file with the following:  
        KKE_ROLE_NAME: name of the role.  
        KKE_POLICY_NAME: name of the policy.  
    Use terraform.tfvarsfile to input the role and policy names.  
    Use outputs.tf file to output the following:  
        kke_iam_role_name: name of the role created.  
        kke_iam_policy_name: name of the policy ceated.  

Solution:  
`main.tf`:
```terraform
resource "aws_iam_role_policy" "test_policy" {
  name = var.KKE_POLICY_NAME
  role = aws_iam_role.test_role.id

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "ec2:Describe*",
        ]
        Effect   = "Allow"
        Resource = "*"
      },
    ]
  })
}

resource "aws_iam_role" "test_role" {
  name = var.KKE_ROLE_NAME

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Sid    = ""
        Principal = {
          Service = "ec2.amazonaws.com"
        }
      },
    ]
  })
}
```
`variables.tf`:
```terraform
variable "KKE_ROLE_NAME" {
  type = string
}

variable "KKE_POLICY_NAME" {
  type = string
}
```
`terraform.tfvars`:
```terraform
KKE_POLICY_NAME = "devops-policy"
KKE_ROLE_NAME = "devops-role"
```
`outputs.tf`:
```terraform
output "kke_iam_role_name" {
  value       = aws_iam_role.test_role.name
}

output "kke_iam_policy_name" {
  value       = aws_iam_policy.test_policy.name
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
