The Nautilus DevOps team is currently engaged in a cleanup process, focusing on removing unnecessary data and services from their AWS account.  
As part of the migration process, several resources were created for one-time use only, necessitating a cleanup effort to optimize their AWS environment.  
A S3 bucket named nautilus-bck-9820 already exists.  
1) Copy the contents of nautilus-bck-9820 S3 bucket to /opt/s3-backup/ directory on terraform-client host (the landing host once you load this lab).  
2) Delete the S3 bucket nautilus-bck-9820.  
3) Use the AWS CLI through Terraform to accomplish this task—for example, by running AWS CLI commands within Terraform. The Terraform working directory is /home/bob/terraform. Update the main.tf file (do not create a separate .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

Solution:  
```
resource "null_resource" "aws_cli" {
  provisioner "local-exec" {
    command = "aws s3 cp s3://nautilus-bck-9820 /opt/s3-backup/ --recursive && aws s3 rm s3://nautilus-bck-9820 --recursive && aws s3api delete-bucket --bucket nautilus-bck-9820"
  }
}
```

[reference](https://developer.hashicorp.com/terraform/language/resources/provisioners/local-exec)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
