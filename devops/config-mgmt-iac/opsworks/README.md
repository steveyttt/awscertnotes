Chef Automate
Puppet enterprise
AWS Ops Works Stacks (This is what is covered in the exam)

### AWS Ops Works Stacks ###
Uses Chef Solo (Stand alone Chef version)
Deploys Chef cookbooks (recipes)
Manage applications on prem and in the cloud
Declarative engines
Can install packages, control daemons, control files

Manages IAM, TAGs, Monitoring and deployments

Can do time / load based scaling of the underlying nodes

Can import existing EC2 instances into ops Works
Can use own and community cookbooks with version 12
Can use built in cookbooks with version 11

There are 3 layer types:
OpsWorks (Cookie cut templated server deployment)
ECS
RDS

https://docs.aws.amazon.com/opsworks/latest/userguide/best-deploy.html
https://github.com/ACloudGuru/devops/blob/master/02/22/adogguru.zip

Terminology is:
STACK >>> LAYERS >>> APPS

Stack - logical construct that encompasses the whole app stack
Layers - Layers of resources which host the workload
- - Like a load balancer layer and an ec2 instance layer
- - Within the layer you define what chef recipes to apply to the infra
- - These recipes are all on github - https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/deploy/recipes/web.rb
APPS - The code that makes up the application (Deployed onto a layer)

If asked in the exam how to manage an EC2 instance using opsworks just rememer it is CHEF. You can use off the shelf or write custom recipes. If you use custom recipes you can hook them up to a REPO.

BERKSHELF - This is a mechanism to manage dependencies. If you write custom cookbooks which need the dflt cookooks BERKSHELF can manage that for you.