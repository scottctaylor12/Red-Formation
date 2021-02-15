# Red Formation

Red Formation is an AWS CloudFormation template that automatically deploys red team infrastructure in the cloud.

## Overview

* A VPC is deployed with 3 EC2 instances for a staging, post-exploitation, and long-haul C2.
* The EC2 instances are built with the default Amazon Linux 2 AMI.
* Port 80 and 443 is open to the internet for C2 traffic.
* SSH, port 22, is not open. Using the AWS SSM service allows command line access without internet-exposed SSH.
* Public, internet-routable, IP addresses will be automatically assigned to each EC2 instance.

## Prerequisites

1. AWS CLI \
   https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html
2. AWS CLI Session Manager Plugin \
   https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html

## Deploying/Deleting Infrastructure

From the command line, run the following command to start the creating infrastructure:  
```
aws cloudformation deploy --stack-name Red --template-file red.yml --capabilities CAPABILITY_NAMED_IAM
```

From the command line, run the following command to *destroy* the infrastructure:  
```
aws cloudformation delete-stack --stack-name Red
```

After deploying the infrastructure, run the following command to list the EC2 instances available for C2: 
```
aws ec2 describe-instances --query 'Reservations[*].Instances[0].{Name:Tags[?Key==`Name`].Value|[0],Instance:InstanceId,IP:PublicIpAddress, State:State.Name}' --output table
```

## Accessing Infrastructure

**No need for internet facing SSH systems or jump boxes!** \
Simply use AWS Systems Manager (SSM) and the AWS CLI to interact with your infrastructure.

Access any of the EC2 instances by using the AWS CLI through the `aws ssm` command:  
```
aws ssm start-session --target <instance id>
```

Use SSM to port forward to your local machine: 
```
aws ssm start-session --target <instance id> --document-name AWS-StartPortForwardingSession --parameters "portNumber"=["80"],"localPortNumber"=["1234"]
```
*Note: This is helpful if your C2 has a web management interface that you need to access locally.*

## Details

There are 3 phases to the cloudformation template:  
1. Core Networking
2. EC2 instances
3. Lambda redirectors (_TODO_)

The following AWS components are used throughout each of the phases.

*Core Networking*
* VPC
* InternetGateway
* Subnets
* Route Table

*EC2 Instances*  
* IAM
* Security Groups
* EC2
* EBS

*Lambda Redirectors*  
_TODO_