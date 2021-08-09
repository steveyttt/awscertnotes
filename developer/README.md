### General ###
Pagination
if you see "timed out" or too many results returned, you can potentially use pagination

General:
- session state can be stored in ```elasticache``` and ```dynamo```
- - RDS can store it too, but it is a bit naf

- Secure connection strings for lambda can be stored in:
- - ```env vars```
- - ```S3```
- - ```Parameter Store```


Elasticache
- ```redis``` can do sorted sets, these are compute intense actions that are great for things like gaming leader boards

### How does enterprise auth work for SSO ###
The enterprise user accesses the identity broker application. The identity broker application authenticates the users against the corporate identity store. The identity broker application has permissions to access the AWS Security Token Service (STS) to request temporary security credentials. Enterprise users can get a temporary URL that gives them access to the AWS APIs or the Management Console.

### AWS APP SYNC ###
- unified API which can be used to access multiple different data sources across one api call (SQL, NoSQL, lambda etc)
- Develop graphQL apis

### CLI ###
https://aws.amazon.com/premiumsupport/knowledge-center/authenticate-mfa-cli/

IAM CLI
- If you see ```Encoded authorization failure message```, you can decrypt the encoded message with ```aws sts decode-authorization-message```.
- The ```context key``` in IAM is the conditional value / element inside the IAM policy statement. i.e. username, sourceIP, date
- - https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html
- - https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html
- - You can use the ```aws iam simulate-custom-policy``` command to test a policy 

AWS credentials hierachy order process:
- Env Vars ```AWS_ACCESS_KEY_ID``` && ```AWS_SECRET_ACCESS_KE```
- Java system properties
- Web Identity Token credentials from the environment or container.
- ~/.aws/credentials - ```DEFAULT ONE FOR OS```
- Amazon ECS container credentials
- Instance Profile Credentials

Todo:

recap all notes - TWICE
run a full quiz from acloudguru - DONE
- identify blind spots and fix them (Dynamo, APIgw)
Run a test with the practice 10 questions from AWS - DONE

Play with services inside the console
- API GW (priority) 
- - https://aws.amazon.com/getting-started/hands-on/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/module-1/
- - https://github.com/ACloudGuru-Resources/course-aws-certified-developer-associate/tree/main/Serverless_Webite_Demo
- - https://aws.amazon.com/getting-started/hands-on/build-web-app-s3-lambda-api-gateway-dynamodb/?trk=gs_card
- -     
- Lambda (priority) 
- - https://github.com/aws-samples/aws-serverless-workshops
- EBS (priority) Deploy an elastic beanstalk demo app (they exist in the console), there is a link in the cloudguru piece too
- - https://workshop.aws-management.tools/xray/
- ECS (priority) 
- - https://ecsworkshop.com/
- SAM
- - https://cicd.serverlessworkshops.io/
- XRAY (With docker and with EC2) (priority) 
- - https://workshop.aws-management.tools/xray/
- - https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon.html
- - https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon-ecs.html
- code commit / build / pipeline (priority)
- - https://aws.amazon.com/getting-started/hands-on/create-continuous-delivery-pipeline/?trk=gs_card
- - https://github.com/ACloudGuru-Resources/course-aws-certified-developer-associate/tree/main/Code_Pipeline_Demo
- - https://aws.amazon.com/getting-started/hands-on/migrate-git-repository/
- Brush up on ELBs
- Cognito
- - https://github.com/aws-samples/aws-cognito-quicksight-auth
- SQS
- - https://aws.amazon.com/getting-started/hands-on/send-messages-distributed-applications/

Look at the links below
read doco associated with key services (including elasticache)
Study dynamo more
buy a practice test - done
book the real exam





Notes from the exam:









