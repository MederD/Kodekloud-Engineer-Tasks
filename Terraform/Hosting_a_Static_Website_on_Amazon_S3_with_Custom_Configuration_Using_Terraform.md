The Nautilus DevOps team has been tasked with creating an internal information portal for public access. As part of this project, they need to host a static website on AWS using an S3 bucket.  
The S3 bucket must be configured for public access to allow external users to access the static website directly via the S3 website URL.  
Your task is to create a Terraform module named s3-static-site to handle the creation and configuration of the S3 bucket. For uploading the index.html file, you may use either Terraform or the AWS CLI.  
Task Requirements:  
The module directory /home/bob/terraform/modules/s3-static-site/ is already created, configure the module to perform the following tasks:  
    Create an S3 bucket named xfusion-web-16521.  
    Configure the S3 bucket for static website hosting with index.html as the index document.  
    Allow public access to the bucket by attaching the appropriate bucket policy.  
    Within the module, use a variables.tf file that must define the following variables: bucket_name and index_document. These values should not be hardcoded directly into resource definitions.  
    You may add other variables if needed to avoid hardcoding. Use these variables in main.tf for configuring the bucket.  
    Within the module use outputs.tf file to output the following:  
        website_url: S3 static website url  
    Your S3 website url should look something like the following, aws:4566 refers to the mock AWS endpoint configured in your environment (e.g., using LocalStack):  
        http://aws:4566/BUCKET_NAME/index.html  
    The S3 bucket must be tagged with the key Project and the value StaticWeb.  
    In the root main.tf, call the s3-static-site module using the required input variables (bucket_name, index_document).  
    Upload the index.html file from /home/bob/terraform directory to the S3 bucket. This can be done using either the AWS CLI or Terraform (aws_s3_object).  


Solution:  
`module main.tf:`  
```terraform
resource "aws_s3_bucket" "example" {
  bucket = var.bucket_name

  tags = {
    Name = var.bucket_name
    Project = "StaticWeb"
  }
}

resource "aws_s3_bucket_public_access_block" "example" {
  bucket = aws_s3_bucket.example.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_website_configuration" "example" {
  bucket = aws_s3_bucket.example.id
  index_document {
    suffix = var.index_document
  }
}

resource "aws_s3_bucket_policy" "allow_public" {
  bucket = aws_s3_bucket.example.id

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = "*"
        Action = "s3:GetObject"
        Resource = "${aws_s3_bucket.example.arn}/*"
      }
    ]
  })
}
```
`root main.tf:`  
```terraform
locals {
    KKE_BUCKET_NAME = "xfusion-web-16521"
    KKE_INDEX_DOC = "index.html"
}

resource "aws_s3_object" "object" {
  depends_on = [module.s3-static-site.aws_s3_bucket_example]
  bucket = local.KKE_BUCKET_NAME
  key    = local.KKE_INDEX_DOC
  source = "/home/bob/terraform/index.html"
}

module "s3-static-site" {
    source = "./modules/s3-static-site"
    bucket_name = local.KKE_BUCKET_NAME
    index_document = local.KKE_INDEX_DOC
}

output "site_bucket_name" {
  value = module.s3-static-site.bucket_name
}

output "site_website_url" {
  value = module.s3-static-site.website_url
}

```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_object)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
