### Code pipeline

Needs an IAM serice role to function
Can use custom and default KMS keys
Can use a default S3 bucket or a custom S3 bucket in the same account and region

Sources allowed are:
- IF USING VCS YOU NEED TO SPECIFY THE BRANCH!!!! ONE BRANCH PER CODE PIPELINE!!!!
- AWS CodeCommit (This is not supported in codedeploy)
- Amazon ECR
- Amazon S3
- Bitbucket
- GitHub (V1, V2 and Enterprise Server)
- - Can set detection options (Triggers):
- - - Can use CW events to trigger builds when REPO commits happen
- - - Can use CodePipeline to periodically poll the repo for changes (You DONT specify the polling frequency)
- - - For Github you can use github webhooks when changes occur

Set a build stage
- Use code build to build the relevant software (If needed)

set a deploy stage
- use code deploy to deploy to the nodes

### Unit tests inside pipeline
- Add a stage to the pipeline after source
- Add a action name and choose an action provider
- - There are many providers (code build, jenkins, CD, code deploy, beanstalk, ECS, invoke lambda)
- - - There is an action provider called Test which has seections for many tools (code build, device farm, jenkins, blazemeter, ghost inspector UI testing, MF stormrunner load, runscope API monitoring)

### Additional ###
You can add approval stages to pipelines
https://github.com/natonic/Developer-Tools-Deep-Dive/tree/master/Labs/CodePipelineWithManualApproval

## Artifacts ##
Can be stored inside S3 only really. You can set buckets and paths and choose to zip or not.
Ensure you are comfortable with the S3 permissions model when using S3 for artifacts
AWS has a serice called ARTIFACT which is nothing to do with code build artifacts

## Deployment strategies ##
- single target (one server)
- all at once (across many servers) (Not production friendly)
- minimum in-service deployment (specify a minimum health count)
- - allows automated testing and has no downtime
- - quicker than a rolling as it happens in two stages usually
- rolling (roll a select # of nodes in batches)
- - needs health checks
- - can be the longest deployment process
- - generally prevents downtime
- - can be paused to enable testing on new nodes
- blue green 
- - advanced orchestration tooling
- - very rapid deployment process
- - cutover is a DNS change (Big bang switch)
- - green env can be tested before switching DNS
- A/B testing
- Red Black
- - same as B/G, just a different name
- A/B testing (Switching small parts of the app to test their functionality, like feature toggles)
- Canary, open up to a small subset of traffic. Like A/B to a degree. Can be dome With R53 weighted policies.