The Nautilus DevOps team is setting up monitoring in their AWS account. As part of this, they need to create a CloudWatch alarm.  
Using Terraform, perform the following:  
Task Details:  
    Create a CloudWatch alarm named `devops-alarm`.  
    The alarm should monitor `CPU utilization` of an EC2 instance.  
    Trigger the alarm when CPU utilization exceeds `80%`.  
    Set the evaluation period to `5 minutes`.  
    Use a `single` evaluation period.  
Ensure that the entire configuration is implemented using Terraform. The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:  
```
resource "aws_cloudwatch_metric_alarm" "devops-alarm" {
  alarm_name                = "devops-alarm"
  comparison_operator       = "GreaterThanThreshold"
  evaluation_periods        = 1
  period                    = 300        
  metric_name               = "CPUUtilization"
  namespace                 = "AWS/EC2"
  statistic                 = "Average"
  threshold                 = 80
}
```
Check:  
```
aws cloudwatch describe-alarms --alarm-names devops-alarm
```
It will return something like:  
```
{
    "MetricAlarms": [
        {
            "AlarmName": "devops-alarm",
            ...
            "MetricName": "CPUUtilization",
            "Namespace": "AWS/EC2",
            "Statistic": "Average",
            "Dimensions": [],
            "Period": 300,
            "EvaluationPeriods": 1,
            "Threshold": 80.0,
            "ComparisonOperator": "GreaterThanThreshold",
            ...
        }
    ],
    ...
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_metric_alarm#argument-reference)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)


