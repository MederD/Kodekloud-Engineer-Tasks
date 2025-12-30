The Nautilus DevOps Team is working on setting up a new web server for a critical application. The team lead has requested you to create an EC2 instance that will serve as a web server using Nginx.  
This instance will be part of the initial infrastructure setup for the Nautilus project. Ensuring that the server is correctly configured and accessible from the internet is crucial for the upcoming deployment phase.  
As a member of the Nautilus DevOps Team, your task is to create an EC2 instance with the following specifications:  
Instance Name: The EC2 instance must be named datacenter-ec2.  
AMI: Use any available Ubuntu AMI to create this instance.  
User Data Script: Configure the instance to run a user data script during its launch. This script should:  
    Install the Nginx package.  
    Start the Nginx service.  
Security Group: Ensure that the instance allows HTTP traffic on port 80 from the internet.  

Solution:  
```bash
ami=$(aws ec2 describe-images --owners 099720109477 --filters 'Name=name,Values=ubuntu/images/hvm-ssd-gp3/ubuntu-noble-24.04-amd64-server-*' --query 'Images | sort_by(@, &CreationDate) | [-1].ImageId' --output text)
vi script.txt:
`
#!/bin/bash
sudo apt update -y
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
`
aws ec2 run-instances --image-id $ami --instance-type t2.micro --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=datacenter-ec2}]' --user-data file://script.txt
sg_id=$(aws ec2 describe-instances --query "Reservations[].Instances[].SecurityGroups[].GroupId" --output text)
aws ec2 authorize-security-group-ingress --group-id $sg_id --protocol tcp --port 80 --cidr 0.0.0.0/0
pip=$(aws ec2 describe-instances --query "Reservations[].Instances[].PublicIpAddress" --output text)
curl -I http://$pip
```

[reference](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)
