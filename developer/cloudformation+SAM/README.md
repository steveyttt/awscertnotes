Transform section:
Used to store reference code in S3 which you can reference in your CF stack.

### Nested stacks ###
Templates you can call from CF templates.
There is a specifc CF resources (```type: AWS::CloudFormation::Stack```), where you can use the ```TemplateURL``` property to reference an external template. 
- This resource type takes parameters 
- has SNS notification associations too. 
- ```TemplateURL``` MUST be in an S3 bucket.
- It creates a new CF stack using the templateURL stack and assigned parameters (This is known as a ```child``` stack)
- [Github links here](https://github.com/natonic/CloudFormation-Deep-Dive/tree/master/Labs/NestedStacks)
- Keep in mind you need ```CAPABILITY AUTO EXPAND``` when deploying a nested stack


### Sam ###
[SAMPACKAGE])https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-package.html) 
- Used to package lambda code and upload to an S3 bucket 
- outputs a template you can use for the ```sam deploy command```
- ```sam package --template-file ./mytemplate.yaml --output-template-file sam-template.yaml --s3-bucket {MYS3BUCKET}```

[SAMDEPLOY](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html)
- Used to deploy a lambda function
- ```sam deploy --template-file sam-template.yaml --stack-name {MYSTACKNAME} --capabilities CAPABILITY_IAM```

https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html

### SAM commands ###
https://github.com/ACloudGuru-Resources/course-aws-certified-developer-associate/tree/main/CloudFormation_%26_SAM_Demo

SAM templates should have ```Transform: AWS::Serverless-2016-10-31``` at the top (Before resources are defined).