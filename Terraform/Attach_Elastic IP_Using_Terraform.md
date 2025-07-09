he Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.  
Recently they came up with requirements mentioned below.  
There is an instance named datacenter-ec2 and an elastic-ip named datacenter-ec2-eip in us-east-1 region. Attach the datacenter-ec2-eip elastic-ip to the datacenter-ec2 instance using Terraform only.  
The Terraform working directory is /home/bob/terraform. Update the main.tf file (do not create a separate .tf file) to attach the specified Elastic IP to the instance.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:  
There are several way to do this, one way:  
```
resource "aws_eip_association" "eip_assoc" {
  instance_id   = aws_instance.ec2.id
  allocation_id = aws_eip.ec2_eip.id
}
```
or, directly adding association to `eip` resource:  
```
resource "aws_eip" "ec2_eip" {
 ...
  instance                  = aws_instance.ec2.id
 ...
}
```
If we check before the change state was:  
```
bob@iac-server ~/terraform via 💠 default ➜  terraform state show aws_eip.ec2_eip
# aws_eip.ec2_eip:
resource "aws_eip" "ec2_eip" {
    ...
    instance                 = null
```
and after the change it should look like:  
```
bob@iac-server ~/terraform via 💠 default ➜  terraform state show aws_eip.ec2_eip
# aws_eip.ec2_eip:
resource "aws_eip" "ec2_eip" {
    ...
    instance                 = "i-1855425e46120bb0e"
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip)   
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main) 
