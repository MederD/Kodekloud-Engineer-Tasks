To enable secure inter-service communication, the DevOps team needs to configure access to an SNS topic using IAM roles and policies.  
The objective is to allow EC2 instances to publish messages to the topic using proper permissions and role assumptions. Please complete the following tasks:  
    Create an SNS topic named nautilus-sns-topic.  
    Create an IAM role named nautilus-sns-role with EC2 as the trusted entity.  
    Attach an IAM policy named nautilus-sns-policy that grants permission to publish messages to the SNS topic.  
    Use the main.tf file (do not create a separate .tf file) to provision the sns-topic, role and policy.  
    Create the locals.tfwith the following names:  
        KKE_SNS_TOPIC_NAME:name of the sns topic created.  
        KKE_ROLE_NAME: name of the role created.  
        KKE_POLICY_NAME: name of the policy created.  
    Create the outputs.tf file to the output the following:  
        The name of the SNS topic using the output variable kke_sns_topic_name.  
        The name of the role using the output variable kke_role_name.  
        The name of the policy using the output variable kke_policy_name.  

Solution:  
`main.tf`:
```terraform
resource "aws_sns_topic" "test" {
  name = local.KKE_SNS_TOPIC_NAME
}

resource "aws_iam_role" "test_role" {
  name = local.KKE_ROLE_NAME
  managed_policy_arns = [aws_iam_policy.test_policy.arn]

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

resource "aws_iam_policy" "test_policy" {
  name = local.KKE_POLICY_NAME

  policy = jsonencode({
    Version = "2012-10-17"
    Statement: [
        {
            Sid: "ReadOnly",
            Effect: "Allow",
            Action: [
                "sns:Publish"

            ],
            Resource: "${aws_sns_topic.test.arn}"
        }
    ]
  })
}
```
[reference](https://developer.hashicorp.com/terraform/language/values/locals)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
