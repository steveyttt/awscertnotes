## Tutorials ##
- https://docs.aws.amazon.com/codedeploy/latest/userguide/tutorials.html


## IAM requirements##
EC2 instances which are deployed to by code deploy need an IAM instance profile with code deploy permissions. (IAM profile for EC2)
Code deploy needs a servicerole also to deploy to instances (IAM service role for code deploy)

EC2 instances need the code deploy agent installed and running as a service. Example userdata for an instance doing this is here - https://github.com/ACloudGuru/devops/blob/master/01/04/userdata.txt

## Structure within Code deploy ##

Create an application 
- define if it is EC2/On-prem or a lambda function

Create a deployment group, 
- this needs a service IAM role (With code deploy permissions)
- Choose here between in-place and Blue/Green deployments
- - With Blue green you specify the source ASG to use for the deployment
- - You also specify the amount of time to keep the old ASG alive
- Select EC2 instances (By tag) or ASGs to deploy 
- Choose deployment model 
- - All at once
- - half at a time
- - one at a time
- - OR create a deployment config file, where you set % or number of healthy nodes to keep in service
- Select the local balancer target
- You can configure deployment triggers to send notifications to SNS topics for deployment and instance statuses
- You can add alarms which when triggered automatically stop the deployment
- You can configure to rollback when a deployment fails or when alarms are breached

Perform a deploy (Under an application)
- Choose your deployment group
- Choose between Amazon S3 or Github as the artifact store
- - (When you use code pipeline you can choose code commit as a source) (You cannot do it in the code deploy console directly)
- - S3 choice would look like this s3://acg-devops/apetguru.zip
- - You can also perform deployment group overrides to override the deployment group config

## code deploy config file is in folder apetguru ##

Notes:
- When using an ASG deployment group, CodeDeploy automatically installs the code on new nodes as they come online