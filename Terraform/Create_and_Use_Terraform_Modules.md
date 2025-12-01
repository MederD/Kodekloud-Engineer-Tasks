The Nautilus DevOps team is implementing a production-grade, event-driven system using Terraform with workspaces and modules. The goal is to teach advanced Terraform concepts. The requirements are as follows:  
1) Use two Terraform workspaces: dev and prod.  
2) Implement two Terraform modules:  
    network module to create a VPC and a subnet.  
    compute module to create an EC2 instance in the subnet.  
3) Use a locals block in the root moduleto define:  
    A common name prefix: datacenter-${terraform.workspace}.  
    Default tags with keys Project = datacenter and Environment = terraform.workspace.  
4) Use main.tf file to define all resources in a structured and modular way, ensuring clarity and maintainability across modules and workspaces.  
5) Use variables.tf file from the root module with the following variable names:  
    KKE_VPC_CIDR:cidr block for the vpc.(10.0.0.0/16)  
    KKE_INSTANCE_TYPE: EC2 instance type.  
6) Use validation in the variables.tf file to ensure that KKE_INSTANCE_TYPE only acceptst3.micro or t3.large, and display an appropriate error message if any other value is provided.  
7) The modules must merge the incoming tags with resource-specific Name tags.  
8) Use dev.tfvarsand prod.tfvars files with the following:  
    In dev.tfvars: KKE_INSTANCE_TYPE= t3.micro  
    In prod.tfvars: KKE_INSTANCE_TYPE =t3.large  
9) Use outputs.tf file from the root module with the following output names:  
    kke_vpc_name: Name of the created VPC.  
    kke_subnet_name: Name of the created Subnet.  
    kke_instance_name: Name of the created EC2 instance.  
10) Network Module:  
Use variables.tf file from the network module with the following variable names:  
    KKE_NAME_PREFIX: Name prefix to use for network resources.  
    KKE_VPC_CIDR: CIDR block for the VPC.  
    KKE_TAGS: Common tags map for network resources.  
11) Use outputs.tf file from the network module with the following output names:  
    kke_vpc_name: Name of the created VPC.  
    kke_subnet_name: Name of the created Subnet.  
12) Compute Module:  
Use the Amazon Linux 2 AMI image with ID ami-0c94855ba95c71c99 for the EC2 instance in the compute module.  
13) Use variables.tf file from the compute module with the following variable names:  
    KKE_NAME_PREFIX: Name prefix to use for compute resources.  
    KKE_SUBNET_ID: Subnet ID where the instance will be created.  
    KKE_INSTANCE_TYPE: EC2 instance type.  
    KKE_TAGS: Common tags map for compute resources.  
14) Use outputs.tf file from the compute module with the following output names:  
    kke_instance_name: Name of the created EC2 instance.  

Solution:  
`main.tf`  
```terraform
locals {
  common_prefix = "datacenter-${terraform.workspace}"
  default_tags = {
    Project     = "datacenter"
    Environment = terraform.workspace
  }
}
module "network" {
  source         = "./modules/network"
  KKE_NAME_PREFIX = local.common_prefix
  KKE_VPC_CIDR   = var.KKE_VPC_CIDR
  KKE_TAGS       = local.default_tags
}
module "compute" {
  source             = "./modules/compute"
  KKE_NAME_PREFIX    = local.common_prefix
  KKE_SUBNET_ID      = module.network.kke_subnet_id
  KKE_INSTANCE_TYPE  = var.KKE_INSTANCE_TYPE
  KKE_TAGS           = local.default_tags
}
```
`variables.tf`
```terraform
variable "KKE_VPC_CIDR" {
  type        = string
  default     = "10.0.0.0/16"
}
variable "KKE_INSTANCE_TYPE" {
  type        = string
  validation {
    condition     = contains(["t3.micro", "t3.large"], var.KKE_INSTANCE_TYPE)
    error_message = "KKE_INSTANCE_TYPE must be: t3.micro, t3.large."
  }
}
```
`outputs.tf`
```terraform
output "kke_vpc_name" {
  value = module.network.kke_vpc_name
}
output "kke_subnet_name" {
  value = module.network.kke_subnet_name
}
output "kke_instance_name" {
  value = module.compute.kke_instance_name
}
```
`network/main.tf`
```terraform
resource "aws_vpc" "this" {
  cidr_block = var.KKE_VPC_CIDR
  tags = merge(
    var.KKE_TAGS,
    { Name = "${var.KKE_NAME_PREFIX}-vpc" }
  )
}
resource "aws_subnet" "this" {
  vpc_id     = aws_vpc.this.id
  cidr_block = "10.0.1.0/24"
  tags = merge(
    var.KKE_TAGS,
    { Name = "${var.KKE_NAME_PREFIX}-subnet" }
  )
}
```
`network/variables.tf`
```terraform
variable "KKE_NAME_PREFIX" {
  type        = string
}
variable "KKE_VPC_CIDR" {
  type        = string
}
variable "KKE_TAGS" {
  type        = map(string)
}

```
`compute/main.tf`
```terraform
resource "aws_instance" "this" {
  ami           = "ami-0c94855ba95c71c99"
  instance_type = var.KKE_INSTANCE_TYPE
  subnet_id     = var.KKE_SUBNET_ID
  tags = merge(
    var.KKE_TAGS,
    { Name = "${var.KKE_NAME_PREFIX}-ec2" }
  )
}
```
`compute/variables.tf`
```terraform
variable "KKE_NAME_PREFIX" {
  type        = string
}
variable "KKE_SUBNET_ID" {
  type        = string
}
variable "KKE_INSTANCE_TYPE" {
  type        = string
}
variable "KKE_TAGS" {
  type        = map(string)
}

```

[reference](https://developer.hashicorp.com/terraform/language/modules)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
