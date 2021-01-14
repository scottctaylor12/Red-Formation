# Red Formation

Red Formation is an AWS CloudFormation template that automatically deploys red team infrastructure in the cloud.

## Prerequisites

In order to run this cloudformation template from your workstation, configure the AWS CLI.
Instructions can be found at: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html

Additionally, you will need the Session Manager Plugin for AWS CLI.
Install instructions can be found at:  
https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html

## Creating/Destroying Infrastructure

From the command line, run the following command to start the creating infrastructure:  
`aws cloudformation deploy --stack-name Red --template-file red.yml --capabilities CAPABILITY_NAMED_IAM`

From the command line, run the following command to *destroy* the infrastructure:  
`aws cloudformation delete-stack --stack-name Red`

## Accessing Infrastructure

No need for SSH on internet facing systems or jump boxes! 
Simply use AWS Systems Manager (SSM) and the AWS CLI to interact with your infrastrucutre.

Access an any of the EC2 instances by using the AWS CLI:  
`aws ssm start-session --target <instance id>`

Use SSM to port forward to your local machine: 
`aws ssm start-session --target <instance id> --document-name AWS-StartPortForwardingSession --parameters "portNumber"=["80"],"localPortNumber"=["56789"]`
 
## Details

There are 3 phases to the cloudformation template:  
1. Core Networking
2. EC2 instances
3. Lambda redirectors

The following AWS components are used throughout each of the phases.

*Core Networking*
* VPC
* InternetGateway
* Subnets
* Route Table

*EC2 Instances*  
_TODO_

*Lambda Redirectors*  
_TODO_