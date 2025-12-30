The Nautilus DevOps team has been tasked with setting up an EC2 instance for their application. To ensure the application performs optimally, they also need to create a CloudWatch alarm to monitor the instance's CPU utilization.  
The alarm should trigger if the CPU utilization exceeds 90% for one consecutive 5-minute period. To send notifications, use the SNS topic named devops-sns-topic which is already created.  
    Launch EC2 Instance: Create an EC2 instance named devops-ec2 using any appropriate Ubuntu AMI.  
    Create CloudWatch Alarm: Create a CloudWatch alarm named devops-alarm with the following specifications:  
        Statistic: Average  
        Metric: CPU Utilization  
        Threshold: >= 90% for 1 consecutive 5-minute period.  
        Alarm Actions: Send a notification to devops-sns-topic.  


Solution:  
```bash
ami=$(aws ec2 describe-images --owners 099720109477 --filters 'Name=name,Values=ubuntu/images/hvm-ssd-gp3/ubuntu-noble-24.04-amd64-server-*' --query 'Images | sort_by(@, &CreationDate) | [-1].ImageId' --output text)
aws ec2 run-instances --image-id $ami --instance-type t2.micro --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=devops-ec2}]'
instance_id=$(aws ec2 describe-instances --query "Reservations[].Instances[].InstanceId" --output text)
topic_arn=$(aws sns list-topics --query "Topics[].TopicArn" --output text)
aws cloudwatch put-metric-alarm --alarm-name devops-alarm --alarm-description "Alarm when CPU exceeds 90 percent" --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 90 --comparison-operator GreaterThanOrEqualToThreshold  --dimensions "Name=InstanceId,Value=$instance_id" --evaluation-periods 1 --alarm-actions $topic_arn --unit Percent
```

[reference1](https://awscli.amazonaws.com/v2/documentation/api/2.1.29/reference/cloudwatch/put-metric-alarm.html)  
[reference2](https://documentation.ubuntu.com/aws/aws-how-to/instances/find-ubuntu-images/)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)
