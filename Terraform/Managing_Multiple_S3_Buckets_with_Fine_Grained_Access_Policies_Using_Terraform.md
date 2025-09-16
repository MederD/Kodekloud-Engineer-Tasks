The Nautilus DevOps team needs to set up three S3 buckets for different environments with backup and policy configurations. Follow the steps below:  
    Create three S3 buckets using for_each for environments: Dev, Staging, and Prod.  
    Name the buckets using the following naming convention:  
        devops-dev-bucket-10207  
        devops-staging-bucket-10207  
        devops-prod-bucket-10207  
    Add the following tags to each bucket with the corresponding values:  
    a.) For devops-dev-bucket-10207:  
        Name = devops-dev-bucket-10207  
        Environment = Dev  
        Owner = Alice  
    b.) For devops-staging-bucket-10207:  
        Name = devops-staging-bucket-10207  
        Environment = Staging  
        Owner = Bob  
    c.) For devops-prod-bucket-10207:  
        Environment = Prod  
        Owner = Carol  
    For the staging and prod buckets, set Backup = true and add a lifecycle rule with ID MoveToGlacier to transition objects to Glacier after 30 days.  
    Use the lifecycle block with ignore_changes to protect the tags.  
    Create a bucket policy that allows public read access to all objects in the bucket.  
    Use depends_on to ensure the policy is only applied after the bucket has been created.  
    Implement the entire configuration in a single main.tf file (do not create a separate .tf file) to provision multiple S3 buckets with the specified configurations.  
    Use variables.tf with the following variable:  
        KKE_ENV_TAGS. KKE_ENV_TAGS is a map that holds environment-specific metadata such as bucket name, owner, and backup flag.  
    Use outputs.tf file to output the following:  
        kke_bucket_names: output the names of the bucket created.   


Solution:  
`main.tf:`
```terraform
resource "aws_s3_bucket" "bucket" {
  for_each = var.KKE_ENV_TAGS
  bucket = each.value.Name
  tags = {
    Name        = each.value.Name
    Environment = each.value.Environment
    Owner       = each.value.Owner
  }

  lifecycle {
    ignore_changes = [tags]
  }

  dynamic "lifecycle_rule" {
    for_each = each.value.Backup ? [1] : []
    content {
      id      = "MoveToGlacier"
      enabled = true

      transition {
        days          = 30
        storage_class = "GLACIER"
      }
    }
  }
}

resource "aws_s3_bucket_policy" "bucket_policy" {
  for_each = aws_s3_bucket.bucket

  bucket = each.value.id

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Sid       = "PublicReadGetObject"
      Effect    = "Allow"
      Principal = "*"
      Action    = ["s3:GetObject"]
      Resource  = ["arn:aws:s3:::${each.value.bucket}/*"]
    }]
  })

  depends_on = [aws_s3_bucket.bucket]
}

```
`variables.tf:`  
```terraform
variable "KKE_ENV_TAGS" {
  type = map(object({
    Name        : string
    Environment : string
    Owner       : string
    Backup      : bool
  }))
  default = {
    Dev = {
      Name        = "devops-dev-bucket-10207"
      Environment = "Dev"
      Owner       = "Alice"
      Backup      = false
    }
    Staging = {
      Name        = "devops-staging-bucket-10207"
      Environment = "Staging"
      Owner       = "Bob"
      Backup      = true
    }
    Prod = {
      Name        = "devops-prod-bucket-10207"
      Environment = "Prod"
      Owner       = "Carol"
      Backup      = true
    }
  }
}

```
`outputs.tf:`  
```terraform
output "kke_bucket_names" {
  value = [for b in aws_s3_bucket.bucket : b.bucket]
}

```
[reference1](https://www.env0.com/blog/terraform-for-each-examples-tips-and-best-practices)  
[reference2](https://developer.hashicorp.com/terraform/language/expressions/dynamic-blocks)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
