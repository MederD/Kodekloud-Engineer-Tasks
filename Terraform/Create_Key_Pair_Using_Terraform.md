The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.  
To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.  
By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.  
For this task, create a key pair using Terraform with the following requirements:  
Name of the key pair should be `devops-kp`.  
Key pair type must be `rsa`.  
The private key file should be saved under `/home/bob`.  
The Terraform working directory is /home/bob/terraform. Create the `main.tf` file (do not create a different .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:  
Create main.tf
```
resource "tls_private_key" "devops_kp" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "local_file" "private_key" {
  content  = tls_private_key.devops_kp.private_key_pem
  filename = "/home/bob/devops-kp.pem"
  file_permission = "0600"
}

resource "aws_key_pair" "devops_kp" {
  key_name   = "devops-kp"
  public_key = tls_private_key.devops_kp.public_key_openssh
}
```
Test  
```
terraform init
terraform validate
terraform plan
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
