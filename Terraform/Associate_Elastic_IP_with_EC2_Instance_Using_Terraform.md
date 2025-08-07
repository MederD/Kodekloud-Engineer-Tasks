The Nautilus DevOps Team has received a new request from the Development Team to set up a new EC2 instance. This instance will be used to host a new application that requires a stable IP address.  
To ensure that the instance has a consistent public IP, an Elastic IP address needs to be associated with it. This setup will help the Development Team to have a reliable and consistent access point for their application.  
    Create an EC2 instance named `devops-ec2` using any Linux AMI like `Ubuntu`.  
    Instance type must be `t2.micro` and associate an Elastic IP address named `devops-eip` with this instance.  
    Use the main.tf file (do not create a separate .tf file) to provision the EC2-Instance and Elastic IP.  
    Use the outputs.tf file and output the instance name using variable `KKE_instance_name` and the Elastic IP using variable `KKE_eip`.  

Solution:  
`main.tf`:  
```terraform
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["amazon"]
  filter {
    name   = "Name"
    values = ["ubuntu/images/hvm-ssd/*"]
  }
}

resource "aws_instance" "devops_ec2" {
  ami                    = data.aws_ami.ubuntu.id
  instance_type          = "t2.micro"
  tags = {
    Name = "devops-ec2"
  }
}

resource "aws_eip" "devops_eip" {
  instance = aws_instance.devops_ec2.id
}
```
`outputs.tf`:  
```terraform
output "KKE_instance_name" {
  value = aws_instance.devops_ec2.tags["Name"]
}

output "KKE_eip" {
  value = aws_eip.devops_eip.public_ip
}
```
[reference](https://medium.com/@knoldus/fetch-the-latest-ami-in-aws-using-terraform-4ea9b95025d7)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  

