The Nautilus DevOps team needs to set up a DynamoDB table for storing user data. They need to create a DynamoDB table with the following specifications:  
1) The table name should be devops-users.  
2) The primary key should be devops_id (String).  
3) The table should use PAY_PER_REQUEST billing mode.  
Use Terraform to create this DynamoDB table. The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to create the DynamoDB table.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

Solution:  
```
resource "aws_dynamodb_table" "devops-users" {
  name           = "devops-users"
  hash_key       = "devops_id"
  billing_mode   = "PAY_PER_REQUEST"

  attribute {
    name = "devops_id"
    type = "S"
  }
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/dynamodb_table#argument-reference)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
