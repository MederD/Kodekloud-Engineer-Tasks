The javed DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.  
Recently they came up with requirements mentioned below.  
Create an IAM group named iamgroup_javed using terraform.  
The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:  
```
resource "aws_iam_group" "iamgroup_javed" {
  name = "iamgroup_javed"
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_group)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
