Create IAM role named `iam_markrole` using variable named `KKE_iamrole`.  
Solution:  
```
`main.tf`:  
resource "aws_iam_role" "example" {
  name                = var.KKE_iamrole
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
`variables.tf`:
variable "KKE_iamrole" {
  type = string
  default = "iam_markrole"
}
```

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role#assume_role_policy-1)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
