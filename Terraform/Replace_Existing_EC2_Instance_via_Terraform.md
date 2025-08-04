To test resilience and recreation behavior in Terraform, the DevOps team needs to demonstrate the use of the -replace option to forcefully recreate an EC2 instance without changing its configuration. Please complete the following tasks:  
    Use the Terraform CLI -replace option to destroy and recreate the EC2 instance xfusion-ec2, even though the configuration remains unchanged.  
    Ensure that the instance is recreated successfully.  

Solution:  
```
terraform apply -replace="aws_instance.web_server"
```
Example outputs you will see after above command:  
```bash
Plan: 1 to add, 0 to change, 1 to destroy.

Changes to Outputs:
  ~ instance_id = "i-b42bcbb8408d24a77" -> (known after apply)
---
Apply complete! Resources: 1 added, 0 changed, 1 destroyed.

Outputs:

instance_id = "i-7adf2a9f346205331"
```
[reference](https://developer.hashicorp.com/terraform/cli/state/taint)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main) 
