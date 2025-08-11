The monitoring team wants to improve observability into the streaming infrastructure. Your task is to implement a solution using Amazon Kinesis and CloudWatch.  
The team wants to ensure that if write throughput exceeds provisioned limits, an alert is triggered immediately.  
As a member of the Nautilus DevOps Team, perform the following tasks using Terraform:  
    Create a Kinesis Data Stream: Name the stream `datacenter-kinesis-stream` with a shard count of `1`.  
    Enable Monitoring: Enable shard-level metrics for the stream to track `ingestion` and `throughput errors`.  
    Create a CloudWatch Alarm: Name the alarm `datacenter-kinesis-alarm` and monitor the `WriteProvisionedThroughputExceeded` metric. The alarm should trigger if the metric exceeds a threshold of `1`.  
    Ensure Alerting: Configure the CloudWatch alarm to detect write throughput issues exceeding provisioned limits.  
    Create the main.tf file (do not create a separate .tf file) to provision the Kinesis stream, CloudWatch alarm, and ensure alerting.  
    Create the outputs.tf file with the following variable names to output:  
        `kke_kinesis_stream_name` for the Kinesis stream name.  
        `kke_kinesis_alarm_name` for the CloudWatch alarm name.  

Solution:  
`main.tf`:  
```terraform
resource "aws_kinesis_stream" "stream" {
  name             = "datacenter-kinesis-stream"
  shard_count = 1
  shard_level_metrics = [
    "IncomingBytes",
    "ReadProvisionedThroughputExceeded",
    "WriteProvisionedThroughputExceeded"
  ]
  stream_mode_details {
    stream_mode = "PROVISIONED"
  }

}

resource "aws_cloudwatch_metric_alarm" "alarm" {
  alarm_name                = "datacenter-kinesis-alarm"
  comparison_operator       = "GreaterThanThreshold"
  evaluation_periods        = 1
  period                    = 300        
  metric_name               = "WriteProvisionedThroughputExceeded"
  namespace                 = "AWS/Kinesis"
  statistic                 = "Sum"
  threshold                 = 1
  alarm_description         = "Alarm if write provisioned throughput is exceeded"
  dimensions                = {
    StreamName              = aws_kinesis_stream.stream.name
  }
  treat_missing_data        = "notBreaching"
}
```

`outputs.tf`:  
```terraform
output "kke_kinesis_stream_name" {
  value = aws_kinesis_stream.stream.name
}

output "kke_kinesis_alarm_name" {
  value = aws_cloudwatch_metric_alarm.alarm.alarm_name
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_metric_alarm)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
