The DevOps team has been tasked with creating a secure DynamoDB table and enforcing fine-grained access control using IAM. This setup will allow secure and restricted access to the table from trusted AWS services only.  
As a member of the Nautilus DevOps Team, your task is to perform the following using Terraform:  
    Create a DynamoDB Table: Create a table named devops-table with minimal configuration.  
    Create an IAM Role: Create an IAM role named devops-role that will be allowed to access the table.  
    Create an IAM Policy: Create a policy named devops-readonly-policy that should grant read-only access (GetItem, Scan, Query) to the specific DynamoDB table and attach it to the role.  
    Create the main.tf file (do not create a separate .tf file) to provision the table, role, and policy.  
    Create the variables.tf file with the following variables:  
        KKE_TABLE_NAME: name of the DynamoDB table  
        KKE_ROLE_NAME: name of the IAM role  
        KKE_POLICY_NAME: name of the IAM policy  
    Create the outputs.tf file with the following outputs:  
        kke_dynamodb_table: name of the DynamoDB table  
        kke_iam_role_name: name of the IAM role  
        kke_iam_policy_name: name of the IAM policy  
    Define the actual values for these variables in the terraform.tfvars file.  
    Ensure that the IAM policy allows only read access and restricts it to the specific DynamoDB table created.  

Solution:  
`main.tf`:  
```terraform
resource "aws_dynamodb_table" "table" {
  name           = var.KKE_TABLE_NAME
  hash_key       = "devops_id"
  billing_mode   = "PAY_PER_REQUEST"

  attribute {
    name = "devops_id"
    type = "S"
  }
}

resource "aws_iam_role" "test_role" {
  name = var.KKE_ROLE_NAME
  managed_policy_arns = [aws_iam_policy.test_policy.arn]

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Sid    = ""
        Principal = {
          Service = "dynamodb.amazonaws.com"
        }
      },
    ]
  })
}

resource "aws_iam_policy" "test_policy" {
  name = var.KKE_POLICY_NAME

  policy = jsonencode({
    Version = "2012-10-17"
    Statement: [
        {
            Sid: "ReadOnly",
            Effect: "Allow",
            Action: [
                "dynamodb:GetItem",
                "dynamodb:Query",
                "dynamodb:Scan",

            ],
            Resource: "${aws_dynamodb_table.table.arn}"
        }
    ]
  })
}
```
[reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_examples_dynamodb_specific-table.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
