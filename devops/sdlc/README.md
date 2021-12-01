### Notes from Dev Exam ###

[WhitePaper on CICD from AWS](https://docs.aws.amazon.com/whitepapers/latest/practicing-continuous-integration-continuous-delivery/practicing-continuous-integration-continuous-delivery.pdf)

### code commit ###
- GIT (Source and Version control)
- If you see ```continuous integration``` in the EXAM think of ```code commit```
- Can integrate with SNS and cloudwatch for PR updates (comment, create, update etc)
- Repo permissions are done using IAM
- - Controls exist for Push, Pull, Branch, Merge, PR.
- You create git credentials for users on the IAM screen, under the ```Security Credentials``` tab. 
- - This creates a username and password that you can use with code commit
- - Similar to creating an access key (Can only download creds once and can only have 2 active creds at once)
- - - ```SSH access``` requires you to craete an SSH key and to upload it to the SSH Key screen on the same screen where you can download http creds for code commit.
- You can configure approval templates (minimum PR approvals before a merge can happen)
- - These can specify mandatory approvals from certain local IAM users or ARNs
- - These can be set on specific branches
- Can configure SNS triggers for REPO changes (all / commit / branch specific)
- - This can notify SNS topic or perform lambda changes


Create a REPO with ```aws codecommit create-repository --repository-name RepoFromCLI --repository-description "My demonstration repository"```

### Code Build ###
- Compiles source code, runs tests and produces packages
- If you see ```continuous delivery``` in the EXAM think of ```code build```
- Can store ```env vars``` in ```parameter store```

```buildspec.yaml file``` (You can call it a different name if you use the CLI for deployment)
phases:
  install:
    run-as: Linux-user-name
    on-failure: ABORT | CONTINUE
    runtime-versions:
      runtime: version
      runtime: version
    commands:
      - command
      - command
    finally:
      - command
      - command
  pre_build: ```All commands for pre_build can also be run for build and post_build``` 
      run-as: Linux-user-name
    on-failure: ABORT | CONTINUE
    commands:
      - command
      - command
    finally:
      - command
      - command
  build:
  post_build:


### Code deploy ###
- Deploy to nodes (In and out of cloud)
- works with EC2, On-Prem and Lambda
- - Can't deploy in-place with lambda (Only B/G)
- Deployment models
- - in-place (rolling)
- - Blue / Green
- - - BLUE = CURRENT
- - - GREEN = NEXT RELEASE (performs an LB switch)
- Has integrations with ALBs to take nodes out of service before performing a release
- Application versions are called ```REVISIONS```
- If you see ```continuous delivery``` in the EXAM think of ```code deploy```
- If you see ```continuous deployment``` in the EXAM think of ```code deploy```

### code deploy AppSpec File ###
- Config file which defines the parameters for a Code Deploy deployment
- For ```EC2 supports YAML only```
- For ```Lambda YAML and JSON``` are supported

4 Key sections of file
- Version
- - Reserved for future use
- OS
- - OS details (Windows / Linux etc)
- Files
- - Config files / packages (and their location source / destination)
- - defined with a source and destination format
- ```Hooks``` - VERY IMPORTANT!!!
- - Scripts that run the deployment (And the order)
- - Can do things like Unzip files, run tests, load balancer registrations
- - - When you see ```life cycle event hooks``` in an exam question, this is what they are talking about. They are run in a specific order an example is
- - - - BeforeInstall
- - - - AfterInstall
- - - - ApplicationStart
- - - - ValidateService

```
version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html/WordPress
hooks:
  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/change_permissions.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
    - location: scripts/create_test_db.sh
      timeout: 300
      runas: root
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root
```


Common lifecycle hooks are covered in 3 phases
- Phase 1
- - ```deregister instances from LB```
- Phase 2
- - ```App deployment```
- - - app stop
- - - download bundle
- - - before install
- - - install
- - - after install
- - - app start
- - - validate service
- Phase 3
- - ```Re-register with LB```

The appspec.yaml must be placed in the ```root directory``` of your application revision. Otherwise the deployment will fail.

```revisions```  have specific structures with folders called:
- /scripts
- /config
- /source

You need a ```service role for code deploy``` to use code deploy. (This is an IAM role with permissions to manage the deployment, i.e. drain nodes from load balancers)

Uses ```tags``` for ```deployment targets```

CodeDeploy [handy commands](https://github.com/ACloudGuru-Resources/course-aws-certified-developer-associate/tree/main/Code_Deploy_Demo)

When configuring code deploy you need to:
- Create an application
- Upload an artifact to S3 for the code deploy (This is a zip file with an appspec.yaml and associated deployment binaries)
- Associate the deployment with a ```deployment``` group
- - Deployment group is where you configure:
- - - ```IAM role``` that performs the deploy
- - - Deployment model ```(In-Place or B/G)```
- - - Choose if it is on-prem, EC2 or ASG nodes 
- - - ```Tags``` of nodes to deploy too
- - - rolling deployment model (one at a time, all at once or half at a time)
- - - Load balancer (If needed)
- Once completed you select ```create deployment``` where you select the configured deployment group and configure the source code location (Github or S3)
- - You can configure rollback options too


### Code Pipeline ###
- Orchestrate the end to end deployment process using code commit, code build and code deploy
- links to code commit for triggered builds
- integrates with github, jenkins and many other AWS services (Beanstalk, ECS, lambda)
- [DEMO of CODE PIPELINE](https://github.com/ACloudGuru-Resources/course-aws-certified-developer-associate/tree/main/Code_Pipeline_Demo)
- Managed images (windows, Amazon linux and ubuntu) are used as the build environment. Docker images are used for custom build environments

Pipeline can use a few sources for code:
- AWS CodeCommit
- Amazon ECR
- Amazon S3
- GitHub

Deployments can trigger using CW events or by AWS code pipeline periodically polling for changes.

Build provider (Optional step)
- Can use CodeBuild or Jenkins

pipeline Deployment providers can be:
- AWS CloudFormation
- AWS Code Deploy
- Elastic Beanstalk
- Serice Catalog
- Skills Kit
- ECS
- ECS Blue Green
- Amazon S3

```Each pipeline build links to one branch of your REPO!!!!```

pipeline stages can have manual approval steps added. (i.e. approve before deploy).
- An SNS topic an be integrated here optionally to notify people to approve the deploy

A file called ```buildspec.yaml``` can be used to configure the deployment parameters / commands associated with a ```pipeline deployment```. You can also ```insert build commands``` yourself into the pipeline configuration outside of a buildspec.yaml using the code pipeline console.

- You can add environment variables to deployments.
- Environment variables can also be encrypted by ```parameter store```

All ``build`` logs go into ``cloudwatch logs``.

### AppSPEC.yaml == code deploy ###

### BuildSPEC = codebuild###


- If you need to migrate a large REPO to code commit (over 2GB, over 5 years old, many large images), then you can do it in stages using the:
- - ```incremental-repo-migration.py```
- If you migrate a normal REPO to code commit you can just do a git push and use the ```git push ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyClonedRepository --tags --all``` --tags and --all commands.


code deploy phases are:
- ```Application stop```
- download bundle
- before install
- install
- after install
- application start
- ```validate service```

Code commit
- Cross account or cross region REPO permissions can be created using role assumption.
- - https://docs.aws.amazon.com/codecommit/latest/userguide/cross-account.html
- - Create a cross account role, add the sts assume role permission to the remote developers

CICD
- https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-create-cross-account.html
- To configure cross account pipelines you need 
- - A KMS key which has permissions to be used by both accounts
- - A cross account role