The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.  
To achieve this, they have segmented large tasks into smaller, more manageable units.  
For this task, create an EC2 instance using Terraform with the following requirements:  
    The name of the instance must be `xfusion-ec2`.
    Use the Amazon Linux `ami-0c101f26f147fa7fd` to launch this instance.
    The Instance type must be `t2.micro`.
    Create a new RSA key named `xfusion-kp`.
    Attach the `default` (available by default) security group.
    The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to provision the instance.
    Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal. 

Solution:  
```
resource "tls_private_key" "xfusion_kp" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = tls_private_key.xfusion_kp.public_key_openssh
}

data "aws_security_group" "default" {
  name = "default"
}

resource "aws_instance" "xfusion_ec2" {
  ami                    = "ami-0c101f26f147fa7fd"
  instance_type          = "t2.micro"
  key_name               = aws_key_pair.xfusion_kp.key_name
  vpc_security_group_ids = [data.aws_security_group.default.id]

  tags = {
    Name = "xfusion-ec2"
  }
}
```

reference:  
  - [RSA key](https://registry.terraform.io/providers/hashicorp/tls/latest/docs/resources/private_key)  
  - [ec2](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#vpc_security_group_ids-1)    

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
