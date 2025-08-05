The Nautilus DevOps team wants to provision multiple EC2 instances in AWS using Terraform. Each instance should follow a consistent naming convention and be deployed using a modular and scalable setup.  
Use Terraform to:  
Create `3` EC2 instances using the `count` parameter.  
Name each EC2 instance with the `prefix datacenter-instance` (e.g., datacenter-instance-1).  
Instances should be `t2.micro`.  
The key named should be `datacenter-key`.  
Create main.tf file (do not create a separate .tf file) to provision these instances.  
Use variables.tf file with the following:  
`KKE_INSTANCE_COUNT`: number of instances.  
`KKE_INSTANCE_TYPE`: type of the instance.  
`KKE_KEY_NAME`: name of key used.  
`KKE_INSTANCE_PREFIX`: name of the instnace.  
Use the locals.tf file to define a local variable named `AMI_ID` that retrieves the latest Amazon Linux 2 AMI using a data source.  
Use terraform.tfvars to assign values to the variables.  
Use outputs.tf file to output the following:  
`kke_instance_names`: names of the instances created.  

Solution:  
`main.tf`:  
```terraform
resource "tls_private_key" "key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "aws_key_pair" "key" {
  key_name   = var.KKE_KEY_NAME
  public_key = tls_private_key.key.public_key_openssh
}

resource "aws_instance" "instance" {
  count = var.KKE_INSTANCE_COUNT
  key_name      = aws_key_pair.key.key_name
  ami           = local.AMI_ID
  instance_type = var.KKE_INSTANCE_TYPE
  tags = {
    Name = "${var.KKE_INSTANCE_PREFIX}-${count.index + 1}"
  }
}
```
`variables.tf`:  
```terraform
variable "KKE_INSTANCE_COUNT" {
  type = number
  default = 3
}

variable "KKE_INSTANCE_TYPE" {
  type = string
  default = "t2.micro"
}

variable "KKE_KEY_NAME" {
  type = string
  default = "datacenter-key"
}

variable "KKE_INSTANCE_PREFIX" {
  type = string
  default = "datacenter-instance"
}
```
`locals.tf`:  
```terraform
data "aws_ami" "latest_amazon_linux" {
  most_recent = true
  owners      = ["amazon"]
  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}

locals {
  AMI_ID   = data.aws_ami.latest_amazon_linux.id
}
```
`terraform.tfvars`:  
```terraform
KKE_INSTANCE_COUNT  = 3
KKE_INSTANCE_TYPE   = "t2.micro"
KKE_KEY_NAME        = "datacenter-key"
KKE_INSTANCE_PREFIX = "datacenter-instance"
```
`outputs.tf`:  
```terraform
output "kke_instance_names" {
  description = "Names of the EC2 instances created"
  value       = [for i in aws_instance.instance : i.tags["Name"]]
}
```
[reference](https://developer.hashicorp.com/terraform/language/meta-arguments/count)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
