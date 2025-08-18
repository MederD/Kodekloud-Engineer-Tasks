The Nautilus DevOps team is implementing a messaging system in AWS. They want to create an SNS topic and an SQS queue. The team needs to subscribe the SQS queue to the SNS topic so that any messages sent to the SNS topic will be delivered to the SQS queue.  
    Create an SNS topic named datacenter-sns-topic.  
    Create an SQS queue named datacenter-sqs-queue.  
    Subscribe the SQS queue to the SNS topic.  
    Use the main.tf file (do not create a separate .tf file) to provision the SNS topic and SQS queue.  
    Create the outputs.tf file, and use the following:  
        The ARN of the SNS topic using the output variable kke_sns_topic_arn.  
        The URL of the SQS queue using the output variable kke_sqs_queue_url.  

Solution:  
`main.tf`:  
```terraform
resource "aws_sns_topic" "sns" {
  name = "datacenter-sns-topic"
}

resource "aws_sqs_queue" "sqs" {
  name = "datacenter-sqs-queue"
}

resource "aws_sns_topic_subscription" "sub" {
  topic_arn = aws_sns_topic.sns.arn
  protocol  = "sqs"
  endpoint  = aws_sqs_queue.sqs.arn
}
```
`outputs.tf`:
```terraform
output "kke_sns_topic_arn" {
  value       = aws_sns_topic.sns.arn
}

output "kke_sqs_queue_url" {
  value       = aws_sqs_queue.sqs.url
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sqs_queue#attribute-reference)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
