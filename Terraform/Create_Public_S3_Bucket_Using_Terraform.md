As part of the data migration process, the Nautilus DevOps team is actively creating several S3 buckets on AWS. They plan to utilize both private and public S3 buckets to store the relevant data.  
Given the ongoing migration of other infrastructure to AWS, it is logical to consolidate data storage within the AWS environment as well.  
    Create a public S3 bucket named `nautilus-s3-26999` using Terraform.  
    Ensure the bucket is `accessible publicly` once created by setting the proper ACL.  
    The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
Notes:  
    Create the resources only in the us-east-1 region.  
    Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  
    The name of the S3 bucket should be based on nautilus-s3-26999.  
    You can use the ACL settings to ensure the bucket is publicly accessible.  

Solution:  
```
resource "aws_s3_bucket" "nautilus-s3-26999" {
  bucket = "nautilus-s3-26999"

  tags = {
    Name        = "nautilus-s3-26999"
  }
}

resource "aws_s3_bucket_public_access_block" "nautilus-s3-26999" {
  bucket = aws_s3_bucket.nautilus-s3-26999.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_acl" "nautilus-s3-26999" {
  depends_on = [
    aws_s3_bucket_public_access_block.nautilus-s3-26999
  ]
  bucket = aws_s3_bucket.nautilus-s3-26999.id
  acl    = "public-read"
}
```

check:  
```
aws s3api get-public-access-block --bucket nautilus-s3-26999
```
which will return:  
```
{
    "PublicAccessBlockConfiguration": {
        "BlockPublicAcls": true,
        "IgnorePublicAcls": true,
        "BlockPublicPolicy": true,
        "RestrictPublicBuckets": true
    }
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_acl)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main) 
