When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls.  
The Nautilus DevOps team is currently in the process of configuring these resources and has outlined the following requirements:  
For this task, create an IAM user named iamuser_john using terraform. The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:
```
resource "aws_iam_user" "iamuser_john" {
  name = "iamuser_john"
  tags = {
    Name = "iamuser_john"
  }
}
```

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_user#path-1)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
