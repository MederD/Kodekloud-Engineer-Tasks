The Nautilus DevOps team has been tasked to build a real-time data pipeline on AWS. The pipeline must collect streaming data, stage it in S3, monitor delivery failures, and alert via email. Your task is to implement this end-to-end using Terraform.  
Pipeline Requirements:  
1) Kinesis Firehose:  
    Create a delivery stream named devops-firehose.  
    It should deliver data to an S3 bucket as a staging area.  
2) S3 Bucket:  
    Create a bucket named devops-staging-11808 (value to come from variables).  
    Set private ACL and allow Firehose to write objects into it.  
3) IAM Role and Policy:  
    Create a role with least privilege to allow Firehose to write to the staging bucket.  
4) CloudWatch Alarm:  
    Create a cloudwatch Alarm named devops-firehose-failures.  
    Monitor the Firehose delivery failures metric (DeliveryToS3.Failures) and trigger when failures occur.  
5) SNS Topic:  
    Create a topic devops-alert-topic and link the CloudWatch alarm to it.  
6) SES Email Identity:  
    Create an SES email identity named devops@example.comand verify an SES email identity using an email address provided in the variables.  
7) SNS Subscription:  
    Subscribe the verified SES email identity to the SNS topic to receive notifications.  
8) Use variables.tf file with the following variables:  
    KKE_STAGING_BUCKET_NAME:name of the s3 bucket for staging data.  
    KKE_ALERT_EMAIL:Email address to receive alerts through ses.  
9) Use terraform.tfvarsto input the value of the variables used in the variables.tf.  
10) Use outputs.tf file to output the following:  
    kke_staging_bucket_name:name of the bucket used.  
    kke_firehose_name:name of the firehose delivery stream used.  
    kke_sns_topic_name:name of the sns topic used.  
    kke_cloudwatch_alarm_name:name of the cloudwatch used.  
    kke_ses_identity:name of the ses identity used.

Solution:  
`main.tf:`  
```terraform
resource "aws_s3_bucket" "kke_staging_bucket" {
  bucket = var.KKE_STAGING_BUCKET_NAME
  acl    = "private"
  tags = {
    Name = "kke-staging-bucket"
  }
}

resource "aws_iam_role" "kke_firehose_role" {
  name = "kke-firehose-role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect = "Allow",
        Principal = {
          Service = "firehose.amazonaws.com"
        },
        Action = "sts:AssumeRole"
      }
    ]
  })
}

resource "aws_iam_role_policy" "kke_firehose_policy" {
  name = "kke-firehose-s3-policy"
  role = aws_iam_role.kke_firehose_role.id

  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect = "Allow",
        Action = [
          "s3:PutObject",
          "s3:PutObjectAcl",
          "s3:GetBucketLocation",
          "s3:ListBucket"
        ],
        Resource = [
          aws_s3_bucket.kke_staging_bucket.arn,
          "${aws_s3_bucket.kke_staging_bucket.arn}/*"
        ]
      },
      {
        Effect = "Allow",
        Action = [
          "logs:PutLogEvents"
        ],
        Resource = "*"
      }
    ]
  })
}

resource "aws_kinesis_firehose_delivery_stream" "kke_firehose" {
  name        = "devops-firehose"
  destination = "extended_s3"

  extended_s3_configuration {
    role_arn   = aws_iam_role.kke_firehose_role.arn
    bucket_arn = aws_s3_bucket.kke_staging_bucket.arn
  }
}

resource "aws_sns_topic" "kke_alert_topic" {
  name = "devops-alert-topic"
}

resource "aws_ses_email_identity" "kke_ses_identity" {
  email = var.KKE_ALERT_EMAIL
}

resource "aws_sns_topic_subscription" "kke_sns_subscription" {
  topic_arn = aws_sns_topic.kke_alert_topic.arn
  protocol  = "email"
  endpoint  = var.KKE_ALERT_EMAIL
}

resource "aws_cloudwatch_metric_alarm" "kke_firehose_failures" {
  alarm_name          = "devops-firehose-failures"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1
  metric_name         = "DeliveryToS3.Failures"
  namespace           = "AWS/Firehose"
  period              = 60
  statistic           = "Sum"
  threshold           = 0
  alarm_description   = "Triggers when Firehose fails to deliver data to S3"
  dimensions = {
    DeliveryStreamName = aws_kinesis_firehose_delivery_stream.kke_firehose.name
  }
  alarm_actions = [aws_sns_topic.kke_alert_topic.arn]
}
```
`variables.tf:`  
```terraform
variable "KKE_STAGING_BUCKET_NAME" {}
variable "KKE_ALERT_EMAIL" {}
```
`terraform.tfvars:`  
```terraform
KKE_STAGING_BUCKET_NAME = "devops-staging-11808"
KKE_ALERT_EMAIL         = "devops@example.com"
```
`outputs.tf:`   
```terraform
output "kke_staging_bucket_name" {
  value       = aws_s3_bucket.kke_staging_bucket.bucket
}

output "kke_firehose_name" {
  value       = aws_kinesis_firehose_delivery_stream.kke_firehose.name
}

output "kke_sns_topic_name" {
  value       = aws_sns_topic.kke_alert_topic.name
}

output "kke_cloudwatch_alarm_name" {
  value       = aws_cloudwatch_metric_alarm.kke_firehose_failures.alarm_name
}

output "kke_ses_identity" {
  value       = aws_ses_email_identity.kke_ses_identity.email
}

```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kinesis_firehose_delivery_stream)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
