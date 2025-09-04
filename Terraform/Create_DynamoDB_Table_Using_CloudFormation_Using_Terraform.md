The Nautilus DevOps team wants to automate infrastructure provisioning using CloudFormation. As part of the stack setup, they need to create a DynamoDB table.  
    Create a CloudFormation stack named xfusion-dynamodb-stack.  
    The stack must create a DynamoDB table named xfusion-cf-dynamodb-table.  
    Use the main.tf file (do not create a separate .tf file) to provision a CloudFormation stack and DynamoDB table. Make sure to add a lifecycle block in main.tf to ignore changes to the parameters attribute.  
    Use the variables.tf file with the following variable names:  
        KKE_DYNAMODB_TABLE_NAME: Dynamodb table name.  
    The locals.tf file is already provided and includes the following:  
        cf_template_body: A local variable that stores the CloudFormation template body.  
    Use the outputs.tf file to output the following:  
        KKE_stack_name: CloudFormation stack name  


Solution:  
`main.tf`:
```terraform
resource "aws_cloudformation_stack" "stack" {
  name = "xfusion-dynamodb-stack"
  template_body = local.cf_template_body
  tags = {
    Name =  "xfusion-dynamodb-stack"
  }
}
```
`locals.tf`:  
```terraform
...
"TableName": "${var.KKE_DYNAMODB_TABLE_NAME}",
...
```
`variables.tf`:
```terraform
variable "KKE_DYNAMODB_TABLE_NAME" {
    default = "xfusion-cf-dynamodb-table"
}
```
`outputs.tf`:
```terraform
output "KKE_stack_name" {
    value = aws_cloudformation_stack.stack.tags["Name"]
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudformation_stack#attribute-reference)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
