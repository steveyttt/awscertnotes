inline policies should be treated as one off policies for unique scenarios.
Use managed policies as a preference.

a deny in a policy ALWAYS over writes an assigned allow permission.

You can apply roles on the fly to running instances.

Role and policy changes take place immediately.

Local IAM users can enable MFA inside the "Security credentials" tab when you are looking at the IAM user.

To enable MFA from the CLI:
aws iam create-virtual-mfa-device --virtual-mfa-device-name EC2-User --outfile /home/ec2-user/QRCode.png --bootstrap-method QRCodePNG

aws iam enable-mfa-device --user-name EC2-User --serial-number arn:aws:iam::"USERNUMBERHERE":mfa/EC2-User --authentication-code-1 "CODE1HERE" --authentication-code-2 "CODE2HERE"

You can enforce MFA on CLi actions. To do this check out this link: https://aws.amazon.com/premiumsupport/knowledge-center/authenticate-mfa-cli/
- You will need the sts service for this to work.

IAM has a "credentials report" section. You can download AWS account users and their status (MFA enabled, last time creds used)

STS
Secure Token Service
- Temporary creds you can use to perform actions

STS works for:
- Federated access (SAML)
- Cross account access
- Feration with mobile apps (Open ID)

Federation
- combining a list of users in one domain (Like IAM) with a list of usres in another domain (Like Active Directory).

works with this flow:
- auth to identity store (AD, FB)
- identity broker then calls GetFederationToken
- - This includes duration and IAM policy
- STS confirms policies are correct and provides AWS style creds (Access / secret / token / duration)
- When using sts token the receiving service will check the provided token against IAM

4 x log types for security
cloudtrail
config
cloudwatch logs
VPC flow logs

The IAM:PassRole allows any affected entity to pass roles to AWS services or Accounts, granting them permission to assume the role.
- passing a role to another AWS account
- Passing a role to an AWS service for temp permissions
- Associating a role with an EC2 instance.

When getting (Secure token service) STS creds when federating with AD you use the AssumeRoleWithSaml api call.




When using Web Identity Federation and Cognito to allow a user to access an AWS service (such as an S3 bucket), which of the following is the correct order of steps?

A user authenticates with Facebook first. They are then given an ID token by Facebook. An API call, AssumeRoleWithWebIdentity, is then used in conjunction with the ID token. A user is then granted temporary security credentials.
