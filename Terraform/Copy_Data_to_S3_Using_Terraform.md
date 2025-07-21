The Nautilus DevOps team is presently immersed in data migrations, transferring data from on-premise storage systems to AWS S3 buckets. They have recently received some data that they intend to copy to one of the S3 buckets.  
S3 bucket named devops-cp-20134 already exists. Copy the file /tmp/devops.txt to s3 bucket devops-cp-20134 using Terraform. The Terraform working directory is /home/bob/terraform. Update the main.tf file (do not create a separate .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:  
```
resource "null_resource" "aws_cli" {
  provisioner "local-exec" {
    command = "aws s3 cp /tmp/devops.txt s3://devops-cp-20134/devops.txt"
  }
}
```
check:  
```
bob@iac-server ~/terraform via 💠 default ➜  aws s3 ls s3://devops-cp-20134
2025-07-21 18:31:21         27 devops.txt
```

[reference](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
