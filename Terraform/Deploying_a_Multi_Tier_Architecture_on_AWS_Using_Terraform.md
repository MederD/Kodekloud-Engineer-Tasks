The DevOps team needs to build a secure, modular multi-tier AWS infrastructure to support a modern cloud-native application stack using Terraform. As part of this requirement, use only allowed AWS services and ensure secure variable usage.  
As a member of the Nautilus DevOps Team, your tasks are:  
    Create a DynamoDB Table: Provision a table named devops-app-table with minimal configuration.  
    Create an SNS Topic: Set up a topic named devops-app-topic for messaging and notifications.  
    Create an SSM Parameter: Store a sensitive configuration value in AWS SSM Parameter Store under the name /devops/app/config.  
    Create main.tf file (do not create a separate .tf file) to provision a dynamoDB table, sns-topic and ssm parameter.  
    Use variables.tf file with the following:  
        KKE_ENVIRONMENT: devEnvironment.  
        KKE_DYNAMODB_TABLE_NAME: name of dynamodb table.  
        KKE_SNS_TOPIC_NAME: name of the sns topic.  
        KKE_SSM_PARAM_NAME: name of the SSM parameter.  
    Create terraform.tfvars to input the name of the variables.  
    Use outputs.tf file to output the following:  
        kke_dynamodb_table_name: name of the dynamodb table.  
        kke_sns_topic_arn: arn of the sns-topic created.  
        kke_ssm_parameter_name: name of the ssm parameter created.  


Solution:  
`main.tf:`  
```terraform
resource "aws_dynamodb_table" "table" {
  name           = var.KKE_DYNAMODB_TABLE_NAME
  hash_key       = "taskId"
  billing_mode   = "PAY_PER_REQUEST"

  attribute {
    name = "taskId"
    type = "S"
  }
}

resource "aws_sns_topic" "topic" {
  name = var.KKE_SNS_TOPIC_NAME
  tags = {
    Name = var.KKE_SNS_TOPIC_NAME
  }
}

resource "aws_ssm_parameter" "parameter" {
  name  = var.KKE_SSM_PARAM_NAME
  type  = "String"
  value = "value"
  tags = {
    name = var.KKE_SSM_PARAM_NAME
  }
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ssm_parameter#attribute-reference)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
