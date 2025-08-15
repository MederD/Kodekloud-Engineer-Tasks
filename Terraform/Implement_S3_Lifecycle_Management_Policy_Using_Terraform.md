The Nautilus DevOps team is implementing lifecycle policies to manage object storage efficiently in AWS. They want to create an S3 bucket with a specific lifecycle rule that transitions objects to infrequent access (IA) storage after 30 days and deletes them after 365 days.  
    Create an S3 bucket named xfusion-lifecycle-27902.  
    Enable the S3 Versioning on the bucket.  
    Add a lifecycle rule named xfusion-lifecycle-rule with:  
        Transition to STANDARD_IA storage class after 30 days.  
        Expiration of objects after 365 days.  
    Use the main.tf file (do not create a separate .tf file) to provision the S3 bucket.  
    Use the variable name KKE_bucket_name in the outputs.tf file to output the created bucket name.  

Solution:  
`main.tf`:
```terraform
resource "aws_s3_bucket" "bucket" {
  bucket = "xfusion-lifecycle-27902"
}

resource "aws_s3_bucket_versioning" "versioning_example" {
  bucket = aws_s3_bucket.bucket.id
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_lifecycle_configuration" "example" {
  bucket = aws_s3_bucket.bucket.id
  rule {
    id = "xfusion-lifecycle-rule"

    transition {
      days          = 30
      storage_class = "STANDARD_IA"
    }
    expiration {days = 365}
    status = "Enabled"
  }
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_lifecycle_configuration#transition)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
