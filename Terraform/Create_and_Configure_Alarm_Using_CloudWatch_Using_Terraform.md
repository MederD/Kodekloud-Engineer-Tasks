The Nautilus DevOps team has been tasked with setting up an EC2 instance for their application. To ensure the application performs optimally, they also need to create a CloudWatch alarm to monitor the instance's CPU utilization.  
The alarm should trigger if the CPU utilization exceeds 90% for one consecutive 5-minute period. To send notifications, use the SNS topic named nautilus-sns-topic, which is already created.  
    Launch EC2 Instance: Create an EC2 instance named nautilus-ec2 using any appropriate Ubuntu AMI (you can use AMI ami-0c02fb55956c7d316).  
    Create CloudWatch Alarm: Create a CloudWatch alarm named nautilus-alarm with the following specifications:  
        Statistic: Average  
        Metric: CPU Utilization  
        Threshold: >= 90% for 1 consecutive 5-minute period  
        Alarm Actions: Send a notification to the nautilus-sns-topic SNS topic.  
    Update the main.tf file (do not create a separate .tf file) to create a EC2 Instance and CloudWatch Alarm.  
    Create an outputs.tf file to output the following values:  
    KKE_instance_name for the EC2 instance name.  
    KKE_alarm_name for the CloudWatch alarm name  

Solution:  
`main.tf`:  
```terraform
resource "aws_sns_topic" "sns_topic" {
  name = "nautilus-sns-topic"
}

resource "aws_instance" "my_ec2" {
  ami                    = "ami-0c02fb55956c7d316"
  instance_type          = "t2.micro"
  tags = {
    Name = "nautilus-ec2"
  }
  iam_instance_profile = aws_iam_instance_profile.profile.id
}

resource "aws_iam_instance_profile" "profile" {
  role = aws_iam_role.example.name
}

resource "aws_iam_role" "example" {
  name                = "EC2-role"
  managed_policy_arns = [aws_iam_policy.policy.arn]
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Sid    = ""
        Principal = {
          Service = "ec2.amazonaws.com"
        }
      },
    ]
  })
}

resource "aws_iam_policy" "policy" {
  name        = "ec2-policy"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "sns:Publish",
        ]
        Effect   = "Allow"
        Resource = "${aws_sns_topic.sns_topic.arn}"
      }
    ]
  })
}

resource "aws_cloudwatch_metric_alarm" "alarm" {
  alarm_name                = "nautilus-alarm"
  comparison_operator       = "GreaterThanThreshold"
  evaluation_periods        = 1
  period                    = 300        
  metric_name               = "CPUUtilization"
  namespace                 = "AWS/EC2"
  statistic                 = "Average"
  threshold                 = 90
  dimensions                = {
    InstanceId             = aws_instance.my_ec2.id
  }
  treat_missing_data        = "notBreaching"
  alarm_actions            = [aws_sns_topic.sns_topic.arn]
}
```
`outputs.tf`:
```terraform
output "KKE_instance_name" {
  value = aws_instance.my_ec2.tags["Name"]
}

output "KKE_alarm_name" {
  value = aws_cloudwatch_metric_alarm.alarm.alarm_name
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_metric_alarm#alarm_actions-1)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
