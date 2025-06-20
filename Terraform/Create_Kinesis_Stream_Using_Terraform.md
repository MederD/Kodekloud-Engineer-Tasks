The Nautilus DevOps team needs to create an AWS Kinesis data stream for real-time data processing. This stream will be used to ingest and process large volumes of streaming data, which will then be consumed by various applications for analytics and real-time decision-making.
    The stream should be named devops-stream.  
    Use Terraform to create this Kinesis stream.  
    The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
    Note:  
        Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  
        Before submitting the task, ensure that terraform plan returns No changes. Your infrastructure matches the configuration.  

Solution:  
```
resource "aws_kinesis_stream" "devops-stream" {
  name             = "devops-stream"

  stream_mode_details {
    stream_mode = "ON_DEMAND"
  }

}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kinesis_stream)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
