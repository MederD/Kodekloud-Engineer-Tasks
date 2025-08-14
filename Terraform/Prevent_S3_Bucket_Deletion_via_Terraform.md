To ensure secure and accidental-deletion-proof storage, the DevOps team must configure an S3 bucket using Terraform with strict lifecycle protections.  
The goal is to create a bucket that is dynamically named and protected from being destroyed by mistake. Please complete the following tasks:  
    Create an S3 bucket named xfusion-s3-4545.  
    Apply the prevent_destroy lifecycle rule to protect the bucket.  
    Create the main.tf file (do not create a separate .tf file) to provision a s3 bucket with prevent_destroy lifecycle rule.  
    Use the variables.tf file with the following:  
        KKE_BUCKET_NAME: name of the bucket.  
    Use the terraform.tfvars file to input the name of the bucket.  
    Use the outputs.tffile with the following:  
        s3_bucket_name: name of the created bucket.  

Solution:  
`main.tf`:  
```terraform
resource "aws_s3_bucket" "bucket" {
  bucket = var.KKE_BUCKET_NAME
  lifecycle {
    prevent_destroy = true
  }
}
```
`variables.tf`:  
```terraform
variable "KKE_BUCKET_NAME" {
  type    = string
}
```
`outputs.tf`:  
```terraform
output "s3_bucket_name" {
  value       = aws_s3_bucket.bucket.id
}
```
`terraform.tfvars`:  
```terraform
KKE_BUCKET_NAME = xfusion-s3-4545
```
[reference](https://developer.hashicorp.com/terraform/language/meta-arguments/lifecycle#prevent_destroy)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
