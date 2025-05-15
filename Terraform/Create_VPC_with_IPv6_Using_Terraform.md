The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the AWS cloud. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition.  
Their approach involves creating Virtual Private Clouds (VPCs) as the initial step, as they will be provisioning various services under different VPCs.  
For this task, create a VPC named xfusion-vpc in the us-east-1 region with the Amazon-provided IPv6 CIDR block using terraform.  
The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:  
```
resource "aws_vpc" "xfusion_vpc" {
  cidr_block                       = "10.0.0.0/16"
  assign_generated_ipv6_cidr_block = true

  tags = {
    Name = "xfusion-vpc"
  }
}
```

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
