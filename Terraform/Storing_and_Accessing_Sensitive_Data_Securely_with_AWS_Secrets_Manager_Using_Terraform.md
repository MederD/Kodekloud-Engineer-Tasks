he Nautilus DevOps team needs to securely manage sensitive information using AWS Secrets Manager. The task is to create a secret in AWS Secrets Manager using Terraform. Store a database password securely in this secret.  
Ensure the password is passed as a sensitive Terraform variable, and it should not appear in Terraform logs or output without being marked sensitive.  
Requirements:  
    Create an AWS Secrets Manager secret named nautilus-db-password.  
    Store the database password SuperSecretPassword123! in the secret using Terraform.  
    Mark the Terraform variable for the password as sensitive.  
    Do not expose the actual password in Terraform outputs without marking it sensitive.  
    Create main.tf file (do not create a separate .tf file) to provision a Secret and add the database password in it.  
    Use variables.tffile for the following:  
        KKE_DB_PASSWORD: database password stored in secrets manager.  
    Create a terraform.tfvars to input the database password.  
    Use outputs.tf file to output the following:  
        kke_secret_arn: arn of the secret created.  
        kke_secret_string: database password.  

Solution:  
`main.tf:`  
```terraform
resource "aws_secretsmanager_secret" "secret" {
  name = "nautilus-db-password"
}

resource "aws_secretsmanager_secret_version" "example" {
  secret_id     = aws_secretsmanager_secret.secret.id
  secret_string = var.KKE_DB_PASSWORD
}
```
`variables.tf:`  
```terraform
variable "KKE_DB_PASSWORD" {
  sensitive = true
}
```
`outputs.tf:`  
```terraform
output "kke_secret_arn"{
    value = aws_secretsmanager_secret.secret.arn
}

output "kke_secret_string"{
    value = var.KKE_DB_PASSWORD
    sensitive = true
}
```
[reference](https://developer.hashicorp.com/terraform/tutorials/configuration-language/sensitive-variables)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
