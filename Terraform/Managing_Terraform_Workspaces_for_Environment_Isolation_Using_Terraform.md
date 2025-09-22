The DevOps team is tasked with provisioning multiple API Gateway REST APIs and corresponding CloudWatch Log Groups using the following Terraform features:  
    Create two workspaces named dev and prod.  
    Create API Gateways named dev-datacenter-api-1 and prod-datacenter-api-2.  
    Create matching CloudWatch Log Groups named /aws/apigateway/dev-datacenter-api-1 and /aws/apigateway/prod-datacenter-api-2.  
    Use the count meta-argument to create multiple API Gateway REST APIs and matching log groups.  
    Leverage terraform workspaces to differentiate API Gateway names per environment.  
    Use local-exec provisioner to write a confirmation message to a log file once each resource is created.(e.g., Created API Gateway dev-datacenter-api-2 in workspace dev).  
    Create two different files apigateway.log and loggroups.log in /home/bob/terraform to log the creation of each resource in their respective files.  
    Use a list variable KKE_API_NAMES to define API names (e.g., ["datacenter-api-1", "datacenter-api-2"]).  
    Createmain.tf file (do not create a separate .tf file) to provision the api gateway with matching log groups in different workspaces.  
    Use variables.tf file with the following:  
        KKE_API_NAMES = Names of API Gateways to create.   
    Use terraform.tfvars file to input the names of the API Gateways.  
    Use outputs.tf file to output the following in the two different workspces ( devand prod).  
        kke_api_gateway_names= name of the api gateway created.  
        kke_log_group_names= name of the matching logroups created.  

Solution:   
```bash
terraform workspace new dev
```
`main.tf:`  
```terraform
locals {
  env_prefix = terraform.workspace == "dev" ? "dev" : "prod"
  api_names  = [for name in var.KKE_API_NAMES : "${local.env_prefix}-${name}"]
  log_names  = [for name in local.api_names : "/aws/apigateway/${name}"]
}

resource "aws_api_gateway_rest_api" "example" {
  count       = length(local.api_names)
  name        = local.api_names[count.index]

  provisioner "local-exec" {
    command = "echo Created API Gateway ${self.name} in workspace ${terraform.workspace} >> /home/bob/terraform/apigateway.log"
  }
}

resource "aws_cloudwatch_log_group" "example" {
  count             = length(local.api_names)
  name              = local.log_names[count.index]

  provisioner "local-exec" {
    command = "echo Created Log Group ${self.name} in workspace ${terraform.workspace} >> /home/bob/terraform/loggroups.log"
  }
}
```
```bash
terraform worksapce select prod
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/api_gateway_rest_api)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
