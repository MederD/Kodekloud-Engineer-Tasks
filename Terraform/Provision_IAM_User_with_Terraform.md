The Nautilus DevOps team is experimenting with Terraform provisioners. Your task is to create an IAM user and use a local-exec provisioner to log a confirmation message.  
    Create an IAM user named iamuser_james.  
    Use a local-exec provisioner with the IAM user resource to log the message KKE iamuser_james has been created successfully! to a file called KKE_user_created.log under home/bob/terraform.  
    Create the main.tf file (do not create a separate .tf file) to provision an IAM user.  
    Use variables.tf file with the following:  
        KKE_USER_NAME: name of the IAM user.  
    Use terraform.tfvars to input the name of the IAM user. 
    Use outputs.tf file with the following:  
        kke_iam_user_name: name of the IAM user.  

Solution:  
`main.tf`:
```terraform
resource "aws_iam_user" "iamuser" {
  name = var.KKE_USER_NAME
}

resource "null_resource" "example" {
  provisioner "local-exec" {
    command = "echo KKE ${var.KKE_USER_NAME} has been created successfully! >> KKE_user_created.log"
  }
}
```
`variables.tf`:
```terraform
variable "KKE_USER_NAME" {
  type = string
}
```
`terraform.tfvars`:
```terraform
KKE_USER_NAME = "iamuser_james"
```
`outputs.tf`:
```terraform
output "kke_iam_user_name" {
  value = aws_iam_user.iamuser.name
}
```
[reference](https://developer.hashicorp.com/terraform/language/resources/provisioners/local-exec)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
