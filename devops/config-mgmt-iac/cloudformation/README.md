### intrinsic functions ###
functions you can run at deploy time to get values in real time
- !Ref
- !FindInMap
- Fn:Base64 (Used for EC2 userdata stuff)
- Fn:Cidr (returns an array of IPs)
- Fn:GetAtt (Get an attribute from a deployed resource)
- Fn:GetAZs (Lists a regions AZ)
- Fn:ImportValue (Import a value exported from another stack) (CROSS STACK REFERENCES)
- Fn:Join (Append a set of values into one value)
- Fn:Select (Select a value from an array)
- Fn:Split (Opposite of join, split strings into values)
- Fn:Sub (Substitute values in a variable string)
- Fn:Transform (specifies a macro template)
- Fn:Ref (Returns the value of a resource or parameter)

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

### Condition Functions ###
Functions that can be used for conditional logic (i.e. between dev and prod deployments)
- Fn:And
- Fn:Equals
- Fn:If
- Fn:Not
- Fn:Or

### Wait Conditions ###
DependsOn - Set order to build resources
Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: "aassbb"
    DependsOn: GatewayToInternet ###EC2 deployment will not attempt until the GatewayToInternet resources has been deployed

CreationPolicies (Used in EC2 deployments to tell CF that EC2 deployment is done)
CreationPolicy:
  ResourceSignal:
    Count: "3" #Tells CF to get 3 health signals (i.e. from 3 different EC2 instances)
    TimeOut: PT15M #Set the timeout to 15 mins

EC2 instances would send the signal using ```cfn-signal```

### WaitConditions & Handlers ###
Like a crap creation policy (The doco says use creation policies NOT waitconditions)

WAITHANDLE - Pre signed URL for workloads to send API calls to when they are done (You can pass values to this URL which other resources can use within the CF template)
WAITCONDITION - The condition for how many success signals should be sent

HANDLE 
WaitHandle:
  Type: AWS::CloudFormation::WaitConditionHandle
CONDITION
WaitCondition:
  Type: AWS::CloudFormation:WaitCondition
  DependsOn: "WebServerGroup"
  Properties:
    Handle: 
      Ref: WaitHandle
    TimeOut: "300"
    Count: 
      Ref: "WebServerCapacity"
RESOURCE
WebServerGroup:
  Type: AWS::AutoScaling::AutoScalingGroup
  Properties:
    LaunchConfigurationName:
      Ref: "LaunchConfig"
    MinSize: "1"
    MaxSize: "3"
    DesiredCapacity:
      Ref "WebServerCapacity"
    LoadBalancerNames:
      -
        Ref: "Elb"

### Nested Stacks ###
Check a basic example here - https://github.com/natonic/CloudFormation-Deep-Dive/tree/master/Labs/NestedStacks
Deleting the top level stack deletes the children stacks
Put a stack in a stack (Can be used for repetition and to get around limits)
Type: AWS::CloudFormation::Stack
Resources:
  VpcStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: !Sub https://${S3BucketName}.s3.amazonaws.com/vpc.yaml
      TimeoutInMinutes: 20
      Parameters:
        AvailabilityZones:
          Fn::Join:
            - ','
            - !Ref AvailabilityZones
        VPCCidr: !Ref VPCCidr
        VPCName: !Ref VPCName
        PublicSubnet1Cidr: !Ref PublicSubnet1Cidr
        PublicSubnet2Cidr: !Ref PublicSubnet2Cidr

### Deletion Policies ###
A setting which is associated with each resource in a CF template. A way to control what is deleted when a stack is deleted.
Values can be:
- Delete (default)
- Retain (Retains even if stack is deleted)
- Snapshot (Only exists on some EC2, RDS and Redshiftt pieces) (Takes a snapshot before deletion)
EXAMPLE
myS3ucket:
  type: AWS::S3::Bucket
  DeletionPolicy: Retain

### Stack policies ###
Can enable or prevent CF from performing update actions on a CF stack. For example you can prevent CF from changing an RDS instance within a stack you have already deployed.
- No stack policy means all updates are allowed
- Once a policy is applied, it CANNOT BE DELETED
- Once a policy is applied ALL RESOURCES ARE PROTECTED. you need to add this to allow updates:

{
    Statement: [
        {
            Effect: Allow,
            Action: Update:*,
            Principal: *,
            Resource: *
        },
    ]
}

### 4 x update impacts are ###
No interruption
Some Interruption (i.e. a reboot)
Replacement 
Delete

### ChangeSets ###
Diff the changes before they happen
Then execute as needed
Console, CLI and API applicable

### Custom Resources ###
Use the !GetAtt function to get the attribute from the CF custom resource values.

    "AMIInfo": {
      "Type": "Custom::AMIInfo",
      "Properties": {
        "ServiceToken": { "Fn::GetAtt" : ["AMIInfoFunction", "Arn"] }, ####This is the LAMBDA FUNCTION
        "Region": { "Ref": "AWS::Region" },
        "Architecture": { "Fn::FindInMap" : [ "AWSInstanceType2Arch", { "Ref" : "InstanceType" }, "Arch" ] }
      }
    },

###  Exam tips ###
- 'Invalid Value or Unsupported Resource Property' errors appear only if there is a parameter naming mistake or that the property names are unsupported. 
- 'Resource Failed to Stabilize' errors appear because a timeout is exceeded, the AWS service isn't available or is interrupted. 
- any update which fails to rollback could be because of a number of reasons, but the most popular is due to the deployment account having permissions to create stacks, but not to modify or delete stacks. T