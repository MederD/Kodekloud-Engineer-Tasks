The Nautilus DevOps team needs to create an AMI from an existing EC2 instance for backup and scaling purposes. The following steps are required:  
    They have an existing EC2 instance named `devops-ec2`.  
    They need to create an AMI named `devops-ec2-ami` from this instance.  
    Additionally, they need to launch a new EC2 instance named `devops-ec2-new` using this AMI.  
    Update the main.tf file (do not create a different or separate.tf file) to provision an AMI and then launch an EC2 Instance from that AMI.  
    Create an outputs.tf file to output the following values:  
        `KKE_ami_id` for the AMI ID you created.  
        `KKE_new_instance_id` for the EC2 instance ID you created.  

Solution:  
`main.tf`:  
```terraform
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  vpc_security_group_ids = [
    "sg-4b3fdea970740e7dd"
  ]

  tags = {
    Name = "devops-ec2"
  }
}

resource "aws_ami_from_instance" "ami" {
  name               = "devops-ec2-ami"
  source_instance_id = aws_instance.ec2.id
}

resource "aws_instance" "ec2_new" {
  ami           = aws_ami_from_instance.ami.id
  instance_type = "t2.micro"

  tags = {
    Name = "devops-ec2-new"
  }
}
```  
`outputs.tf`:  
```terraform
output "KKE_ami_id" {
  value = aws_ami_from_instance.ami.id
}

output "KKE_new_instance_id" {
  value = aws_instance.ec2_new.id
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ami_from_instance)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  

