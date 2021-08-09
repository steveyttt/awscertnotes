Tags & resource groups
Tag editor allows you to edit tags on many resourcs at once
Case sensitive

resource groups are region based (Not global).
- based on tags or Cf stack
- You can execute actions on resource groups

AWS Cost explorer
- can view data up to 13 months old
- provides forecasting (Up to 3 months)
- Get RI recommendations
- Download CSV reports
- You enable cost allocation tags in "billing support"
- - Examples of good tags for this are ones related to billing show back DEPARTMENT - TEAM 
- - Once enabled you can then view resources with those tags in cost explorer.

service health dashboard
- Health of all AWS services WW
- status.aws.amazon.com

personal health dashboard
- more tailored alarms related to service ouatges and how that affects resources provisioned in your account
- it is regional

services with MAINTENANCE windows are (i.e. automated version updates):
- RDS
- elasticachee
- redshift
- dynamo DN dax
- neptune
- asmazon document DB

Services without maintenance windows are:
- lambda
- ec2
- amazon qldb (qunatm ldger database)

SNOWBALL (Data transfer only)
- 256 bit encryption
- encrypts and copies data
- used when you have many TB or low bandwidth
- - if it takes 7 days ti upload - use snowball

SNOWBALL edge (Data transfer with local data processing options)
- 100TB
- can do edge processing
- S3 compatible end points
- supports NFS
- can run lambda functions (But they need specifying when you order the device)

Storage Gateway:
- on premises appliance (ESXi VMware or Hyper-V)
- connects to AWS storage 
-  Connect with on-prem instanmces via NFS or iscsi

- types of gateway

- - file gateway (NFS SMB)
- - - looks like any other file system, but backed by S3 (Can use all S3 features)
- - - can connect over DX or internet

- - volume gateway (iscsi) (There are 2 types)
- - - gateway stored volumes (store all your data locally and only backup to AWS)
- - - - You need to store all DATA locally
- - - gateway cached volumes (S3 is primary storage and then you cache data locally when it is accessed)
- - - - only need local cache data

- - - tape gateway
- - - - archive data to glacier
- - - - data is stored in virtual tapes (VTL)

Which AWS service allows you to back up data stored in your own data center as EBS snapshots stored in S3?
-  Volume Gateway - Stored volumes

LAMBDA:
You can increase the concurrency limit of a lambda function using the AWs support center.

DYNAMO:
Supports provisioned capacity and also auto scaling of performance

AWS AppSync is used for GraphQL powered applications

SQS
short polling - poll a queue and get a response back quick (Not necessarily all messages)
long polling - poll for longer, get all messages, can run fr up to 20 seconds

Running a --dry-run option in the cli does a --dry-run :)


FAQs to read:
Cloudwatch
Cloudformation
Elastic beanstalk
S3
RDS
Athena
Elasticache
IAM
KMS
inspector
TA
VPC
R53
DX
ELB
EC2
Systems Manager
Config

Whitepapers:
Development and test on AWS
backup and recovery approaches using AWS
https://d1.awsstatic.com/whitepapers/aws-development-test-environments.pdf
https://d0.awsstatic.com/whitepapers/Backup_and_Recovery_Approaches_Using_AWS.pdf
https://d1.awsstatic.com/whitepapers/aws-amazon-vpc-connectivity-options.pdf
https://d1.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf
https://d1.awsstatic.com/whitepapers/aws-security-whitepaper.pdf
https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf
https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf
https://d1.awsstatic.com/whitepapers/AWS_Cloud_Best_Practices.pdf
https://d1.awsstatic.com/whitepapers/aws-web-hosting-best-practices.pdf
https://d1.awsstatic.com/whitepapers/building-a-scalable-and-secure-multi-vpc-aws-network-infrastructure.pdf
