The Nautilus DevOps team is developing a simple 'To-Do' application using DynamoDB to store and manage tasks efficiently. The team needs to create a DynamoDB table to hold tasks, each identified by a unique task ID.  
Each task will have a description and a status, which indicates the progress of the task (e.g., 'completed' or 'in-progress').  
Your task is to:  
Create a DynamoDB table named devops-tasks with a primary key called taskId (string).  
Insert the following tasks into the table:  
Task 1: taskId: 1, description: Learn DynamoDB, status: completed  
Task 2: taskId: 2, description: Build To-Do App, status: in-progress  
Verify that Task 1 has a status of completed and Task 2 has a status of in-progress.  
Create main.tf(do not create a separate .tf file) to provision a dynamo_db table and insert tasks.  
Create a variables.tf file with the following:  
KKE_TABLE_NAME: name of the dynamo_db table.  
Use terraform.tfvars file to input the name of the dynamo_db table.  
Use outputs.tf file for the following:  
kke_dynamodb_table_name: name of the dynamo_db table created.  


Solution:  
`main.tf:`  
```terraform
resource "aws_dynamodb_table" "table" {
  name           = var.KKE_TABLE_NAME
  hash_key       = "taskId"
  billing_mode   = "PAY_PER_REQUEST"

  attribute {
    name = "taskId"
    type = "S"
  }
}

resource "aws_dynamodb_table_item" "task1" {
  table_name = aws_dynamodb_table.table.name
  hash_key   = aws_dynamodb_table.table.hash_key
  item = <<ITEM
{
  "taskId": {"S": "1"},
  "description": {"S": "Learn DynamoDB"},
  "status": {"S": "completed"}
}
ITEM
}

resource "aws_dynamodb_table_item" "task2" {
  table_name = aws_dynamodb_table.table.name
  hash_key   = aws_dynamodb_table.table.hash_key
  item = <<ITEM
{
  "taskId": {"S": "2"},
  "description": {"S": "Build To-Do App"},
  "status": {"S": "in-progress"}
}
ITEM
}
```

[reference](https://notes.kodekloud.com/docs/Terraform-Basics-Training-Course/Terraform-with-AWS/DynamoDB-with-Terraform)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
