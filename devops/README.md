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
CHECK WHIZLABS -  study guide PDF
SOMETHING FUN - https://aws.amazon.com/blogs/devops/using-aws-codepipeline-for-deploying-container-images-to-aws-lambda-functions/
Read the FAQ of all services
Go through the guide and check I know all the things: https://d1.awsstatic.com/training-and-certification/docs-devops-pro/AWS-Certified-DevOps-Engineer-Professional_Exam-Guide.pdf
do an AWS practive test

### WhitePapers ###
This is an important one:
https://d1.awsstatic.com/whitepapers/DevOps/practicing-continuous-integration-continuous-delivery-on-AWS.pdf
https://d1.awsstatic.com/whitepapers/DevOps/Jenkins_on_AWS.pdf
(Look into CICD tools and how Jenkins can be used with the AWS tools)
https://d1.awsstatic.com/whitepapers/AWS_Blue_Green_Deployments.pdf

https://d1.awsstatic.com/whitepapers/DevOps/running-containerized-microservices-on-aws.pdf
https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf
https://d1.awsstatic.com/whitepapers/DevOps/infrastructure-as-code.pdf
https://d1.awsstatic.com/whitepapers/DevOps/import-windows-server-to-amazon-ec2.pdf
https://d1.awsstatic.com/whitepapers/AWS_DevOps.pdf
https://d1.awsstatic.com/whitepapers/aws-development-test-environments.pdf

LAMBDA - Check from developer folder
CONFIG - Check from sysops folder
STEPFUNCTIONS - Check the terminology
WHIZLABS - https://www.whizlabs.com/aws-devops-certification-training/
KINESIS HANDS ON - https://docs.aws.amazon.com/lambda/latest/dg/with-kinesis-example.html


### Did you know? ###
The full range of AWS Trusted Advisor Checks and Recommendations", only two support plans have all the Trust Advisor checks and these are Business and Enterprise

### From the practice test ###
- Trusted Advisor integrates with EventBridge. Check results can trigger lambda functions to perform follow up actions. reresh check and us-east-1 as region.
- command runs before conatiner_comands inside EBS
- Mysql supports cross region read replicas (Good for DR)
- Subscription filters run from cloudwatch logs and can pass "filtered" log group content to a lambda / Elasticsearch / kinesis (stream or firehose)
- TA has an alert for limits at 80% (and does not guarantee when all checks run)
- launch configs contain the AMI info. ASGs contain the scaling info. Create a ne LC with a new AMI then set the ASG termination policy to ```oldestlaunchconfiguration``` to remove old nodes.
- workflow-driven pipeline actions run multiple tasks, deal with async behavior and loops, need to maintain and propagate state, and fork into different execution paths. if you use lambda - think continuation token!!!
- AddToLoadBalancer - if you suspend this auto scaling will not add new nodes to the LB. They will need to be added manually!
- aws data pipeline is a real service which can transfer data between services (RDS, Dynamo, S3 and EMR)
- you can change an EBS environment from single to load balanced WITHOUT the need to rebuild
- Ops works has 3 instance types 
- - 24/7 (manual)
- - time-based instances (started and stopped on schedule)
- - load-based instances (based on CW metrics)