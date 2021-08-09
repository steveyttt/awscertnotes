### Languages ###
JAVA
.NET
PHP
Node.JS
Python
Ruby
Go
Docker

### Platforms ###
Apache
Tomcat
Nginx
Passenger
IIS
Puma
GlassFish (Java)

- Cloudtrail and cloudwatch integration
- No charge for Elastic Beanstalk (Only resources that it provisions)
- Manages OS and APP server updates
- Creates load balancers, instances, security groups - everything

4 update options for Elastic Bean stalk deployments

ALL AT ONCE
- deploy to all concurrently
- total outage
- rollbacks have an outage too

ROLLING
- deploy in batches
- If you have 4 in-service it takes 2 our of action and update, then bring back into service and update the remaining 2
- Creates reduced capacity during the deployment

ROLLING WITH ADDITIONAL BATCH
- Launches additional batches of instances, then deploys the new version in batches
- Like a rolling update above, but provisions additional instances to keep the in-service instances count the same throughout the deployment
- No reduced performance issues during the deployment with this

IMMUTABLE (Best)
- Deploys a fresh bunch of instances before deleting the old ones
- Deploys to a completely new set of instances, can perform a DNS switch
- fastest mode to switch between versions of code

Traffic splitting
- Pass a small % of traffic to a new version of the app
- Think Canary testing
- deploys a new immutablle instance for a new version of the app. Then splits a small % of traffic to the new node. If the new instances stay healthy then Elastic beanstalk will gradually increase the traffic load %.

### Specifc Configurations ###
You can configure elastic beanstalk with ```elastic beanstalk configuration files``` written in YAML or JSON. Config file must have a ```.config``` extension and must be placed inside the ```.ebextensions``` folder at the ```top level`` of you EBS zip file.
- packages to install
- linux users
- run shell commands
- enable services
- load balancer configuration (i.e. health checks)

### RDS specifics ###
```RDS``` can be deployed inside your Elastic Beanstalk config or separately.
- If you deploy RDS with EBS when you delete your EBS env IT WILL DELETE RDS TOO!
- - There is no option to terminate EC2 but keep RDS
- For production systems it is best to provision RDS outside of the EBS environment. When this is done you should:
- - Configure the SecG on the RDS instance to allow EBS nodes in
- - You will need to use Elastic BeanStalk ```environment properties``` to provide the RDS connection string to EBS. (In the console it is under the Configuration setting)

### Docker ###
- Can run one container on one EC2 instance
```OR```
- use EBS to build an ECS cluster and deploy multiple docker containers on each instance (```multi container docker```)
- To run docker on EBS you just upload your code bundle in ZIP format. (From local desktop or in S3)
- It is a one click process in the console to deploy and rollback
- You can store your code in codecommit. (But you cannot use the EBS CLI)


From the test exam:
https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.managing.ec2.html
https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options.html#configuration-options-recommendedvalues

Elastic Bean Stalk
- You can create custom platform in EBS by building images from instances. The docs even reference packer.
- - https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/custom-platforms.html

AWS Elastic Beanstalk supports a large number of configuration options that let you modify the settings that are applied to resources in your environment. Several of these options have default values that can be overridden to customize your environment. Other options can be configured to enable additional features.

Elastic Beanstalk supports two methods of saving configuration option settings. Configuration files in YAML or JSON format can be included in your application's source code in a directory named .ebextensions and deployed as part of your application source bundle. You create and manage configuration files locally.

Saved configurations are templates that you create from a running environment or JSON options file and store in Elastic Beanstalk. Existing saved configurations can also be extended to create a new configuration.

Elastic Bean Stalk
- Elastic Beanstalk creates an ```application version``` whenever you upload source code. This usually occurs when you ```create an environment or upload and deploy code using the environment management console or EB CLI```. Elastic Beanstalk deletes these application versions according to the ```application's lifecycle policy``` and when you delete the application. For details about application lifecycle policy, see Configuring application version lifecycle settings.
- - https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/applications-versions.html

Elastic Bean Stalk
- If you want to deploy a worker application that processes ```periodic background tasks```, your application source bundle must also include a ```cron.yaml``` file. For more information, see Periodic tasks.