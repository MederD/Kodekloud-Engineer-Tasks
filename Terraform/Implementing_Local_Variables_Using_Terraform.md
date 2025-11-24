The DevOps team is tasked with designing and implementing a production‑grade, event‑driven infrastructure entirely using Terraform. This initiative is part of our internal platform engineering efforts to standardize infrastructure as code (IaC) practices across the organization.  
Task Requirements:  
    Use a locals block in your Terraform configuration to define the following:  
        project name ( nautilus).  
        environment (dev),  
        A common name prefix by combining the project name and environment ( nautilus-dev).  
        A map of default tags ( Project, Environment, Owner, and Team).  
        Then, reference these locals values across all resources in your configuration to ensure consistent naming and consistent tagging.  
    Create a SNS topic named project name-environment-topic.  
    Create a SQS queue named project name-environment-queue and subscribe it to the SNS topic.  
    Use depends_on to ensure the SNS topic is created before subscription.  
    Create a DynamoDB table named project name-environment-events with primary key event_id (HASH key) and provisioned throughput of 5 read capacity units and 5 write capacity units.  
    Create an IAM Role named project name-environment-role with a `dynamic inline policy allowing:  
        sqs:ReceiveMessage  
        dynamodb:PutItem  
        sns:Publish  
    Use validation blocks invariables.tf to:  
        Restrict allowed AWS regions to only us-east-1 (with error message)  
        Ensure the SNS queue depth threshold is between 1 and 1000 (with the error message).  
    Create a CloudWatch alarm named project name-environment-alarm for the SQS queue when it contains more than 50 messages (threshold configurable).  
    Use variables.tf file with the following variables:  
        KKE_AWS_REGION:aws region used.  
        KKE_QUEUE_DEPTH_THRESHOLD:CloudWatch alarm threshold for queue depth.(default=50).  
        KKE_IAM_ACTIONS:IAM actions to allow in dynamic policy.  
    Use outputs.tf file to output the following:  
    kke_cloudwatch_alarm_name:name of the alarm created.  
    kke_dynamodb_table_name:name of the table created.  
    kke_iam_role_arn: arn of the role created.  
    kke_sns_topic_arn: arn of the sns-topic created.  
    kke_sqs_queue_url: url of the sqs-queue created.  


Solution:  
`main.tf`
```terraform
locals {
  project_name    = "nautilus"
  environment     = "dev"
  common_name     = "${local.project_name}-${local.environment}"
    default_tags = {
    Project    = local.project_name
    Environment = local.environment
    Owner      = "iptcp"
    Team       = "IT"
  }
}
resource "aws_sns_topic" "sns_topic" {
  name = "${local.common_name}-topic"
  tags = local.default_tags
}
resource "aws_sqs_queue" "sqs_queue" {
  name = "${local.common_name}-queue"
  tags = local.default_tags
}
resource "aws_sns_topic_subscription" "queue_subscription" {
  endpoint             = aws_sqs_queue.sqs_queue.arn
  protocol             = "sqs"
  topic_arn            = aws_sns_topic.sns_topic.arn
  raw_message_delivery = true
  depends_on = [
    aws_sns_topic.sns_topic,
    aws_sqs_queue.sqs_queue
  ]
}
resource "aws_sqs_queue_policy" "sqs_queue_policy" {
  queue_url = aws_sqs_queue.sqs_queue.url
  policy    = data.aws_iam_policy_document.sqs_policy.json
}
data "aws_iam_policy_document" "sqs_policy" {
  statement {
    actions   = ["SQS:SendMessage"]
    resources = [aws_sqs_queue.sqs_queue.arn]
    principals {
      type        = "Service"
      identifiers = ["sns.amazonaws.com"]
    }
  }
}
resource "aws_dynamodb_table" "dynamo_table" {
  name           = "${local.common_name}-events"
  hash_key       = "event_id"
  billing_mode   = "PROVISIONED"
  read_capacity  = 5
  write_capacity = 5

  attribute {
    name = "event_id"
    type = "S"
  }

  tags = local.default_tags
}
resource "aws_iam_role" "iam_role" {
  name = "${local.common_name}-role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Principal = {
        Service = "sns.amazonaws.com"
      }
      Action = "sts:AssumeRole"
    }]
  })
  tags = local.default_tags
}
resource "aws_iam_role_policy" "inline_policy" {
  name = "${local.common_name}-policy"
  role = aws_iam_role.iam_role.id
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      for action in var.KKE_IAM_ACTIONS : {
        Effect   = "Allow"
        Action   = action
        Resource = "*"
      }
    ]
  })
}
resource "aws_cloudwatch_metric_alarm" "queue_depth_alarm" {
  alarm_name                = "${local.common_name}-alarm"
  comparison_operator       = "GreaterThanOrEqualToThreshold"
  evaluation_periods        = "1"
  metric_name               = "ApproximateNumberOfMessagesVisible"
  namespace                 = "AWS/SQS"
  period                    = "60"
  statistic                 = "Maximum"
  threshold                 = var.KKE_QUEUE_DEPTH_THRESHOLD
  alarm_description         = "Alarm for when the SQS queue depth exceeds the threshold"
  insufficient_data_actions = []
  dimensions = {
    QueueName = aws_sqs_queue.sqs_queue.name
  }
}
```
`variables.tf`  
```terraform
variable "KKE_AWS_REGION" {
  description = "AWS region to be used"
  type        = string
  default     = "us-east-1"
  validation {
    condition     = contains(us-east-1, var.KKE_AWS_REGION)
    error_message = "The AWS region must be one of the following: us-east-1."
  }
}

variable "KKE_QUEUE_DEPTH_THRESHOLD" {
  description = "CloudWatch alarm threshold for queue depth"
  type        = number
  default     = 50
  validation {
    condition     = var.KKE_QUEUE_DEPTH_THRESHOLD >= 1 && var.KKE_QUEUE_DEPTH_THRESHOLD <= 1000
    error_message = "The queue depth threshold must be between 1 and 1000."
  }
}
variable "KKE_IAM_ACTIONS" {
  description = "List of IAM actions to allow in dynamic policy"
  type        = list(string)
  default     = ["sqs:ReceiveMessage", "dynamodb:PutItem", "sns:Publish"]
}
```

[reference](https://developer.hashicorp.com/terraform/language/expressions/dynamic-blocks)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
