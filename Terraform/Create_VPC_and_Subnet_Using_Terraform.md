To ensure proper resource provisioning order, the DevOps team wants to explicitly define the dependency between an AWS VPC and a Subnet. The objective is to create a VPC and then a Subnet that explicitly depends on it using Terraform's depends_on argument.  
Please complete the following tasks:  
    - Create a VPC named devops-vpc.  
    - Create a Subnet named devops-subnet.  
    Ensure the Subnet uses the depends_on argument to explicitly depend on the VPC resource.  
    - Create the main.tf file (do not create a separate .tf file) to provision a VPC and Subnet.  
    - Use variables.tf, define the following variables:  
        KKE_VPC_NAME for the VPC name.  
        KKE_SUBNET_NAME for the Subnet name.  
    - Use terraform.tfvars to input the names of the VPC and subnet.  
    - In outputs.tf, output the following:  
        kke_vpc_name: The name of the VPC.  
        kke_subnet_name: The name of the Subnet.  
        
Solution:  
```
---
`main.tf`:
resource "aws_vpc" "devops_vpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = var.KKE_VPC_NAME
  }
}
resource "aws_subnet" "devops_subnet" {
  cidr_block = "10.0.1.0/24"
  vpc_id     = aws_vpc.devops_vpc.id

  depends_on = [
    aws_vpc.devops_vpc
  ]

  tags = {
    Name = var.KKE_SUBNET_NAME
  }
}
---
`varaibles.tf`:  
variable "KKE_VPC_NAME" {}
variable "KKE_SUBNET_NAME" {}
---
`terraform.tfvars`:  
KKE_VPC_NAME    = "devops-vpc"
KKE_SUBNET_NAME = "devops-subnet"
---
`outputs.tf`:
output "kke_vpc_name" {
  value       = var.KKE_VPC_NAME
}
output "kke_subnet_name" {
  value       = var.KKE_SUBNET_NAME
}

```
[reference](https://developer.hashicorp.com/terraform/language/values/outputs)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

