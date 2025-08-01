The Nautilus DevOps team is expanding their AWS infrastructure and requires the setup of a private Virtual Private Cloud (VPC) along with a subnet.  
This VPC and subnet configuration will ensure that resources deployed within them remain isolated from external networks and can only communicate within the VPC. Additionally, the team needs to provision an EC2 instance under the newly created private VPC.  
This instance should be accessible only from within the VPC, allowing for secure communication and resource management within the AWS environment.  
Create a VPC named xfusion-priv-vpc with the CIDR block 10.0.0.0/16.  
Create a subnet named xfusion-priv-subnet inside the VPC with the CIDR block 10.0.1.0/24 and auto-assign IP option must not be enabled.  
Create an EC2 instance named xfusion-priv-ec2 inside the subnet and instance type must be t2.micro.  
Ensure the security group of the EC2 instance allows access only from within the VPC's CIDR block.  
Create the main.tf file (do not create a separate .tf file) to provision the VPC, subnet and EC2 instance.  
Use variables.tf file with the following variable names:  
KKE_VPC_CIDR for the VPC CIDR block.  
KKE_SUBNET_CIDR for the subnet CIDR block.  
Use the outputs.tf file with the following variable names:  
KKE_vpc_name for the name of the VPC.  
KKE_subnet_name for the name of the subnet.  
KKE_ec2_private for the name of the EC2 instance.  

Solution:  
```
`main.tf`:
resource "aws_vpc" "my_vpc" {
  cidr_block = var.KKE_VPC_CIDR
  tags = {
    Name = "xfusion-priv-vpc"
  }
}

resource "aws_subnet" "my_subnet" {
  cidr_block = var.KKE_SUBNET_CIDR
  vpc_id     = aws_vpc.my_vpc.id
  map_public_ip_on_launch = false

  tags = {
    Name = "xfusion-priv-subnet"
  }
}

resource "aws_security_group" "my_sg" {
  name        = "my-sg"

  ingress {
    from_port   = 0
    to_port     = 0
    protocol    = -1
    cidr_blocks = [aws_vpc.my_vpc.cidr_block]
  }
}

data "aws_ami" "latest_amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}

resource "aws_instance" "my_ec2" {
  ami                    = data.aws_ami.latest_amazon_linux.id
  instance_type          = "t2.micro"
  subnet_id              = aws_subnet.my_subnet.id
  vpc_security_group_ids = [aws_security_group.my_sg.id]

  tags = {
    Name = "xfusion-priv-ec2"
  }
}
---
`outputs.tf`:
output "KKE_vpc_name" {
  value = aws_vpc.my.tags["Name"]
}

output "KKE_subnet_name" {
  value = aws_subnet.my.tags["Name"]
}

output "KKE_ec2_private" {
  value = aws_instance.my.tags["Name"]
}

```
[reference](https://developer.hashicorp.com/terraform/tutorials/configuration-language/outputs)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
