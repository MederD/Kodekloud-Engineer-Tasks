To enable secure retrieval of secrets, the Nautilus DevOps team needs to configure access to a secret in AWS Secrets Manager using IAM roles and policies.  
The objective is to allow EC2 instances to retrieve secrets securely. Please complete the following tasks:  
    Create a secret in AWS Secrets Manager named datacenter-app-secret with the following secret string:  
    {"db_user":"admin","db_pass":"supersecret"}  
    Create an IAM role named datacenter-app-role with EC2 as the trusted entity.  
    Attach an inline IAM policy named datacenter-app-policy that grants permission to retrieve the secret from AWS Secrets Manager.  
    Use the main.tf file (do not create a separate .tf file) to provision the IAM Role and IAM Policy.  
    Create the variables.tf file, ensure the following variables are defined in variables.tf file:  
        KKE_SECRET_NAME for the secret name.  
        KKE_SECRET_VALUE for the secret value.  
        KKE_ROLE_NAME for the IAM role name.  
        KKE_POLICY_NAME for the IAM policy name.  
    Create the outputs.tf file, and use the following:  
        KKE_secret_name: The secret name  
        KKE_role_name: The IAM role name  
        KKE_policy_name: The IAM policy name  

Solution:  
`main.tf`:
```terraform
resource "aws_secretsmanager_secret" "secret" {
  name = var.KKE_SECRET_NAME
}

resource "aws_secretsmanager_secret_version" "example" {
  secret_id     = aws_secretsmanager_secret.secret.id
  secret_string = var.KKE_SECRET_VALUE
}

resource "aws_iam_role_policy" "test_policy" {
  name = var.KKE_POLICY_NAME
  role = aws_iam_role.test_role.id

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "secretsmanager:GetSecretValue",
        ]
        Effect   = "Allow"
        Resource = "${aws_secretsmanager_secret.secret.arn}"
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
variable "KKE_SECRET_VALUE" {
    default = "{\"db_user\":\"admin\",\"db_pass\":\"supersecret\"}"
    type = string
}

variable "KKE_SECRET_NAME" {
    default = "datacenter-app-secret"
    type = string
}

variable "KKE_ROLE_NAME" {
    default = "datacenter-app-role"
    type = string
}

variable "KKE_POLICY_NAME" {
    default = "datacenter-app-policy"
    type = string
}
```
`outputs.tf`:
```terraform
output "KKE_secret_name" {
    value = var.KKE_SECRET_NAME
}

output "KKE_role_name" {
    value = var.KKE_ROLE_NAME
}

output "KKE_policy_name" {
    value = var.KKE_POLICY_NAME
}
```
[reference](https://developer.hashicorp.com/terraform/language/values/variables#map)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
