The Nautilus DevOps team needs to set up CloudWatch logging for their application. They need to create a CloudWatch log group and log stream with the following specifications:  
1) The log group name should be nautilus-log-group.  
2) The log stream name should be nautilus-log-stream.  
Use Terraform to create the CloudWatch log group and log stream. The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

Solution:  
```
resource "aws_cloudwatch_log_group" "nautilus-log-group" {
  name = "nautilus-log-group"
}

resource "aws_cloudwatch_log_stream" "nautilus-log-stream" {
  name           = "nautilus-log-stream"
  log_group_name = aws_cloudwatch_log_group.nautilus-log-group.name
}
```

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_log_stream)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
