TODO:
Build a python project inside CodeBuild
Deploy an app on ASG and Lambda using code pipeline
Read the CodeBuild FAQ and service documentation
Look at "Settings" tab of all Code services
Handy GitHub links - https://github.com/natonic/Developer-Tools-Deep-Dive
Clarify the StepFunction terminology
OPSWORKS - Need to lab this
Do tests - https://www.whizlabs.com/learn/course/aws-devops-certification-training/183/pt
VALIDATE ALL SERVICES WHICH WORK WITH ORGANIZATIONS


LAMBDA - Check from developer folder
CONFIG - Check from sysops folder
STEPFUNCTIONS - Check the terminology
WHIZLABS - https://www.whizlabs.com/aws-devops-certification-training/
KINESIS HANDS ON - https://docs.aws.amazon.com/lambda/latest/dg/with-kinesis-example.html

Adhoc

SCPs apply to root users in node accounts
SCPs do NOT apply to the root account. This always has all permissions.

Secrets Manager:
- Encrypt keys at rest using your own KMS keys
- Ise cli / API / SDK or console
- Replace hard coded credentials with api calls to secrets manager
- Secrets can be rotated automatically on a schedule
- Secrets are classed as "RDS", "none RDS data bases" or other
- Automated rotation of secrets can be done with AWS services
- - It creates a lambda function to do this for you
- You are provided sample code when creating a secret which demonstrates how to access the secret

Amazon Macie:
{{{Not in Sydney yet}}}
Sec service which uses ML to discover, classify and protect sensitive data
- Can idenitfy PII in data
- Provides a dashboard
- Monitors access to data for anomalies
- Only works on S3

Amazon Certificate Manager
Easily provision and manage public / private TLS certificates
- provides central mgmt
- use is audited in Cloudtrail
- Has private CA authority
- integrates with AWS services seamlessly (ALB / CloudFront / API Gateway)
- Can import certificates from other certificate providers
- - Can use DNS or E-mail to validate domain ownership to get a certificate
- Can still import certs into IAM, but it is preferred to use ACM instead