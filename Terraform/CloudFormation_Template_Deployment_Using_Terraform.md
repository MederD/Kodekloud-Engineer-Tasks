The Nautilus DevOps team is working on automating infrastructure deployment using AWS CloudFormation. As part of this effort, they need to create a CloudFormation stack that provisions an S3 bucket with versioning enabled.  
Create a CloudFormation stack named devops-stack using Terraform. This stack should contain an S3 bucket named devops-bucket-670 as a resource, and the bucket must have versioning enabled.  
The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:  
```
resource "aws_cloudformation_stack" "devops-stack" {
  name = "devops-stack"

  parameters = {
    BucketName = "devops-bucket-670"
  }

  template_body = jsonencode({
    Parameters = {
      BucketName = {
        Type        = "String"
        Default     = "devops-bucket-670"
      }
    }

    Resources = {
      devops-bucket-670 = {
        Type = "AWS::S3::Bucket"
        Properties = {
          BucketName = {
            "Ref" = "BucketName"
          },
          VersioningConfiguration = {
            "Status" = "Enabled"
          }
        }
      }
    }
  })
}
```
Check with `aws s3 ls` will return:  
```
bob@iac-server ~/terraform via 💠 default ➜  aws s3 ls
2025-07-02 20:21:52 devops-bucket-670
```

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudformation_stack#template_body-1)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main) 
