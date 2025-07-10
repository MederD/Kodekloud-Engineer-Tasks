The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.  
Recently they came up with requirements mentioned below.  
An IAM user named iamuser_mark and a policy named iampolicy_mark already exists. Use Terraform to attach the IAM policy iampolicy_mark to the IAM user iamuser_mark.  
The Terraform working directory is /home/bob/terraform. Update the main.tf file (do not create a separate .tf file) to attach the specified IAM policy to the IAM user.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:   
```
resource "aws_iam_policy_attachment" "test-attach" {
  name       = "test-attachment"
  users      = [aws_iam_user.user.name]
  policy_arn = aws_iam_policy.policy.arn
}
```

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy_attachment)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main) 
