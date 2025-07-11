Data protection and recovery are fundamental aspects of data management. It's essential to have systems in place to ensure that data can be recovered in case of accidental deletion or corruption.  
The DevOps team has received a requirement for implementing such measures for one of the S3 buckets they are managing.  
The S3 bucket name is xfusion-s3-15688, enable versioning for this bucket using Terraform.  
The Terraform working directory is /home/bob/terraform. Update the main.tf file (do not create a different .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:  
```
resource "aws_s3_bucket_versioning" "versioning_example" {
  bucket = aws_s3_bucket.s3_ran_bucket.id
  versioning_configuration {
    status = "Enabled"
  }
}
```

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_versioning)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
