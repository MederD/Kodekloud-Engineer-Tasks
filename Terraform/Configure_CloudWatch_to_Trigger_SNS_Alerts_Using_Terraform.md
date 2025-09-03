The Nautilus DevOps team is expanding their AWS infrastructure and requires the setup of a CloudWatch alarm and SNS integration for monitoring EC2 instances.  
The team needs to configure an SNS topic for CloudWatch to publish notifications when an EC2 instance’s CPU utilization exceeds 80%. The alarm should trigger whenever the CPU utilization is greater than 80% and notify the SNS topic to alert the team.  
    Create an SNS topic named datacenter-sns-topic.  
    Create a CloudWatch alarm named datacenter-cpu-alarm to monitor EC2 CPU utilization with the following conditions:  
        Metric: CPUUtilization  
        Threshold: 80%  
        Actions enabled  
        Alarm actions should be triggered to the SNS topic.  
    Ensure that the SNS topic receives notifications from the CloudWatch alarm when it is triggered.  
    Update the main.tf file (do not create a different .tf file) to create SNS Topic and Cloudwatch Alarm.  
    Create an outputs.tf file to output the following values:  
    KKE_sns_topic_name for the SNS topic name.  
    KKE_cloudwatch_alarm_name for the CloudWatch alarm name.   

Solution:  
`main.tf`:  
```terraform
resource "aws_sns_topic" "sns_topic" {
  name = "datacenter-sns-topic"
  tags = {
    Name = "datacenter-sns-topic"
  }
}

resource "aws_sns_topic_policy" "default" {
  arn = aws_sns_topic.sns_topic.arn

  policy = data.aws_iam_policy_document.sns_topic_policy.json
}

data "aws_iam_policy_document" "sns_topic_policy" {
  policy_id = "__default_policy_ID"
  statement {
    actions = [
      "SNS:Subscribe",
      "SNS:SetTopicAttributes",
      "SNS:RemovePermission",
      "SNS:Receive",
      "SNS:Publish",
      "SNS:ListSubscriptionsByTopic",
      "SNS:GetTopicAttributes",
      "SNS:DeleteTopic",
      "SNS:AddPermission",
    ]

    effect = "Allow"

    principals {
      type        = "Service"
      identifiers = ["cloudwatch.amazonaws.com"]
    }

    resources = [
      aws_sns_topic.sns_topic.arn,
    ]

    sid = "__default_statement_ID"
  }
}


resource "aws_cloudwatch_metric_alarm" "alarm" {
  alarm_name                = "datacenter-cpu-alarm"
  comparison_operator       = "GreaterThanThreshold"
  evaluation_periods        = 1
  period                    = 300        
  metric_name               = "CPUUtilization"
  namespace                 = "AWS/EC2"
  statistic                 = "Average"
  threshold                 = 80

  treat_missing_data        = "notBreaching"
  alarm_actions            = [aws_sns_topic.sns_topic.arn]
}
```
`outputs.tf`:
```terraform
output "KKE_sns_topic_name" {
  value = aws_sns_topic.sns_topic.tags["Name"]
}

output "KKE_cloudwatch_alarm_name" {
  value = aws_cloudwatch_metric_alarm.alarm.alarm_name
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic_policy#attribute-reference)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
