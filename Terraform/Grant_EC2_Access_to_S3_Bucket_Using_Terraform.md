The Nautilus DevOps team wants to set up EC2 instances that securely upload application logs to S3 using IAM roles.  
    Create an EC2 instance named nautilus-ec2 that can access an S3 bucket securely.  
    Create an S3 bucket named nautilus-logs-28571.  
    Create an IAM role named nautilus-role with a policy named nautilus-access-policy allowing S3 PutObject on the above bucket.  
    Attach the IAM role to the EC2 instance to allow it to upload logs to the bucket.  
    Create the main.tf (do not create a separate .tf file) to provision the EC2, s3, role and policy.  
    Create the variables.tffile to declare the following:  
        KKE_BUCKET_NAME: name of the bucket.  
        KKE_POLICY_NAME: name of the policy.  
        KKE_ROLE_NAME: name of the role.  
    Create the terraform.tfvars file to assign values to variables.  
    Create a data.tf file to fetch the latest Amazon Linux 2 AMI.  

Solution:  
`main.tf`:  
```terraform
resource "aws_instance" "my_ec2" {
  ami                    = data.aws_ami.latest_amazon_linux.id
  instance_type          = "t2.micro"
  tags = {
    Name = "nautilus-ec2"
  }
  iam_instance_profile = aws_iam_instance_profile.profile.id
}

resource "aws_iam_instance_profile" "profile" {
  role = aws_iam_role.example.name
}

resource "aws_s3_bucket" "bucket" {
  bucket = var.KKE_BUCKET_NAME
}

resource "aws_iam_role" "example" {
  name                = var.KKE_ROLE_NAME
  managed_policy_arns = [aws_iam_policy.policy.arn]
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Sid    = ""
        Principal = {
          Service = "ec2.amazonaws.com"
        }
      },
    ]
  })
}

resource "aws_iam_policy" "policy" {
  name        = var.KKE_POLICY_NAME

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "s3:PutObject",
        ]
        Effect   = "Allow"
        Resource = "${aws_s3_bucket.bucket.arn}"
      }
    ]
  })
}
```
`data.tf`:   
```terraform
data "aws_ami" "latest_amazon_linux" {
  most_recent = true
  owners      = ["amazon"]
  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_instance_profile#attribute-reference)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
