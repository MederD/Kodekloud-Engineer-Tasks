The Nautilus DevOps team needs to store sensitive data securely using AWS Secrets Manager. They need to create a secret with the following specifications:  
1) The secret name should be xfusion-secret.  
2) The secret value should contain a key-value pair with username: admin and password: Namin123.  
3) Use Terraform to create the secret in AWS Secrets Manager.  
The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

Solution:  
```
resource "aws_secretsmanager_secret" "xfusion-secret" {
  name = "xfusion-secret"
}

variable "example" {
  default = {
    username = "admin"
    password = "Namin123"
  }

  type = map(string)
}

resource "aws_secretsmanager_secret_version" "example" {
  secret_id     = aws_secretsmanager_secret.xfusion-secret.id
  secret_string = jsonencode(var.example)
}
```
Check:  
```
aws secretsmanager get-secret-value --secret-id xfusion-secret --query SecretString
```

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret_version)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
