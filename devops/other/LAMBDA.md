dead letter queues
async functions are retried twice (if first run fails)
you can configure a dead letter queue (an sns or sqs target) to receieve the failed execution input
(Request ID, ErrorCode, ErrorMessage)

supports Go, Powershell, Node.JS, C#, Python, JAVA

Charged by:
- Number of requests
- their duration
- their assigned memory

Price depends on assigned memory

Price per GB

0.00001667 per GB-second
Function uses 521MB runs for 100ms = 0.5GB x 0.1s = 0.05 GB - second

lambda can be triggered by:
https://docs.aws.amazon.com/lambda/latest/dg/lambda-services.html
Dynamo
Kinesis
SQS
ALB
API Gateway
Alexa
CloudFront
S3
SNS
SES
CF
CW
CodeCommit
CodePipeline
ALSO MANY 3rd PARTY LIBRARIES!!!!

Default lambda IAM role has permissions to create cloudwatch logs
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "logs:CreateLogGroup",
            "Resource": "arn:aws:logs:us-east-1:536514156614:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:us-east-1:ACCOUNT:log-group:/aws/lambda/FUNCTIONNAME:*"
            ]
        }
    ]
}



lambda functions can have aliases. 
- $LATEST is a common example (always the latest version of the code)
- You can give different versions of code different aliases
- different aliases have the alias in the ARN. 

lambda functions can have versions and aliases.

CREATING MULTIPLE VERSIONS OF CODE
deploy code
publish a new version (i.e. version1, version2, version3)
create an alias for that newly published version (i.e. version1 = prod, version2 = test)
Update the code in your lambda function
Then publish a new version
Then create an alias

If you use aliases, uploading latest versions of code may not update the alias in use!

S3ObjectVersion - Can be used as a parameter in CF when deploying a Lamda fuction. Update the version of code in the S3 bucket and then update the CF template to reference that version.

There is a concurrent execution limits:
Limit on number of functions which can run in one region, default is 1000 per region. You can see this in 2 error messages:
- TooManyRequestsException
- HTTP Status Code: 429
- Request throughput limit exceeded
- YOU WILL SEE NEW INVOCATION REQUESTS GET REJECTED
- 1000 is a soft limit on an AWS account for the number of concurrent executions

```Reserved concurrency guarantess that a set # of executions will always be available for your critical function```. Note this applies at a function level. This can act as a limit though and prevents further scaling beyond the reserved concurrency. 

```Provisioned concurrency``` then applies to the reserved concurrency. Provisioned concurrency ensures that any reserved concurrency is warm and ready to run at any time (Like pre-warming)

private lambda:
- needs a private subnet ID
- needs a security group ID
Then it will create an elastic ENI in the subnet

Move a public lambda function into a private VPC (Can be done in console too)
aws lambda update-function-configuration -function-name my-function --vpc-config SubnetIds=subnet-xxxxxx,SecurityGroupIds=sg-51530134

when lambda runs in a ```VPC it needs these IAM permissions```:
ec2:CreateNetworkInterface
ec2:DeleteNetworkInterface
ec2:DescribeNetworkInterface

### Exam reminders ###
If you are integrating lambda - use API Gateway NOT ALB

When you publish a version, the code and most of the settings are locked to maintain a consistent experience for users of that version

Lambda functions can have ```128 MB and 10,240 MB``` of memory

lambda ```concurrency```
- - - Reserved concurrency – Reserved concurrency guarantees the maximum number of concurrent instances for the function. When a function has reserved concurrency, no other function can use that concurrency. There is no charge for configuring reserved concurrency for a function.
- - - - The lambda function cannot use more than their assigned reserved concurrency
- - - - applies to all versions and aliases of a lambda function
- - - Provisioned concurrency – Provisioned concurrency initializes a requested number of execution environments so that they are prepared to respond immediately to your function's invocations. Note that configuring provisioned concurrency incurs charges to your AWS account.
- SAM deployments

- - - ```Canary```: Traffic is shifted in two increments. You can choose from predefined canary options. The options specify the percentage of traffic that's shifted to your updated Lambda function version in the first increment, and the interval, in minutes, before the remaining traffic is shifted in the second increment.
- - - ```Linear```: Traffic is shifted in equal increments with an equal number of minutes between each increment. You can choose from predefined linear options that specify the percentage of traffic that's shifted in each increment and the number of minutes between each increment.

```Hooks``` - Validation Lambda functions that are run before & after traffic shifting in SAM 
PreTraffic: !Ref PreTrafficLambdaFunction
PostTraffic: !Ref PostTrafficLambdaFunction


Lambda
- https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html
- async lambda function is retried twice before the event is discarded. If they do fail, AWS suggest you use a dead letter queue to direct unprocessed requests to SQS
- - look up https://docs.aws.amazon.com/lambda/latest/dg/invocation-async.html  

- Configuring alias routing
Use the ```create-alias``` and ```update-alias``` AWS CLI commands to configure the ```traffic weights``` between two function versions. When you create or update the alias, you specify the traffic weight in the routing-config parameter.
- - https://docs.aws.amazon.com/lambda/latest/dg/configuration-aliases.html#configuring-alias-routing
- - aws lambda ```create-alias``` --name routing-alias --function-name my-function --function-version 1 ```--routing-config``` AdditionalVersionWeights={"2"=0.03}
- - aws lambda ```update-alias``` --name routing-alias --function-name my-function ```--routing-config``` AdditionalVersionWeights={"2"=0.05}
- - aws lambda ```update-alias``` --name routing-alias --function-name my-function --function-version 2 ```--routing-config``` AdditionalVersionWeights={}

- Data stream consumer ```lambda and kinesis```
- If you see scaling issues with lambda and kinesis then think ```data stream consumer```
- - To minimize latency and maximize read throughput, you can create a data stream consumer with enhanced fan-out. Stream consumers get a dedicated connection to each shard that doesn't impact other applications reading from the stream. The dedicated throughput can help if you have many applications reading the same data, or if you're reprocessing a stream with large records. Kinesis pushes records to Lambda over HTTP/2.

- LAMBDA METRIC 
- - ```IteratorAge``` – For event source mappings that read from streams, the age of the last record in the event. The age is the amount of time between when the stream receives the record and when the event source mapping sends the event to the function.

- SAM
- - ```DeploymentPreference``` - Specifies the configurations to enable gradual Lambda deployments. For more information about configuring gradual Lambda deployments, see Deploying serverless applications gradually.
- - ```AutoPublishALias``` - Creates a new alias and points new version to it. Updates all event sources to use the new alias too. (fastest deployment model)


### Exam ###
- LAMBDA DOES NOT MONITOR MEMORY????
- You can set up a custom CloudWatch metric filter using a pattern that includes 'REPORT', 'MAX', 'Memory' and 'Used:'.