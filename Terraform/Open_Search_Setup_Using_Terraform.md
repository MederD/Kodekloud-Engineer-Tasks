The Nautilus DevOps team needs to set up an Amazon OpenSearch Service domain to store and search their application logs. The domain should have the following specification:  
1) The domain name should be devops-es.  
2) Use Terraform to create the OpenSearch domain. The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
Notes:  
    The Terraform working directory is /home/bob/terraform.  
    Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  
    Before submitting the task, ensure that terraform plan returns No changes. Your infrastructure matches the configuration.  
    The OpenSearch domain creation process may take several minutes. Please wait until the domain is fully created before submitting.

Solution:  
```
resource "aws_opensearch_domain" "devops-es" {
  domain_name    = "devops-es"
}
```

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/opensearch_domain)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
