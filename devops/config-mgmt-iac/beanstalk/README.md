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

With customizations questions with EBS think EBEXTENSIONS

You can use a custom docker image if you need a language / platform not natively supported by EBS

Put everything in a ZIP files and pass it to Elastic beanstalk

EBS actually uses cloudformation in the backend..(You can see the stack in the console after a deployment)

Look in these examples - https://github.com/awsdocs/elastic-beanstalk-samples && https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/php-hawordpress-tutorial.html

### .ebextensions ###
IMPORTANT FOR THE EXAM
Enabled
customization for the EBS setup
- YAML or JSON formatted
- placed in a folder called .ebextensions in the ZIP file containing the EBS code

### Specifc Configurations ###
You can configure elastic beanstalk with ```elastic beanstalk configuration files``` written in YAML or JSON. Config files must have a ```.config``` extension and must be placed inside the ```.ebextensions``` folder at the ```top level`` of you EBS zip file. You can have many .config files in the .ebextensions folder
- packages to install
- linux users
- run shell commands
- enable services
- load balancer configuration (i.e. health checks)

https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ebextensions.html
Example ```.config``` file which goes inside the .ebextensions folder:

files:
  "c:/temp/configureUpdates.ps1":
    content: |
      Invoke-Expression "reg add 'HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update' /v AUOptions /t REG_DWORD /d 4 /f"
      Invoke-Expression "sc config wuauserv start=auto"
      Invoke-Expression "net start wuauserv"
container_commands:
  01-execute-config-scirpt:
    command: Powershell.exe -ExecutionPolicy Bypass -File c:\\temp\\configureUpdates.ps1
    ignoreErrors: true
    waitAfterCompletion: 0

###########
###########

packages:
  yum:
    perl-DateTime: []
    perl-Sys-Syslog: []
    perl-LWP-Protocol-https: []
    perl-Switch: []
    perl-URI: []
    perl-Bundle-LWP: []

sources: 
  /opt/cloudwatch: https://aws-cloudwatch.s3.amazonaws.com/downloads/CloudWatchMonitoringScripts-1.2.1.zip
  
container_commands:
  01-setupcron:
    command: |
      echo '*/5 * * * * root perl /opt/cloudwatch/aws-scripts-mon/mon-put-instance-data.pl `{"Fn::GetOptionSetting" : { "OptionName" : "CloudWatchMetrics", "DefaultValue" : "--mem-util --disk-space-util --disk-path=/" }}` >> /var/log/cwpump.log 2>&1' > /etc/cron.d/cwpump
  02-changeperm:
    command: chmod 644 /etc/cron.d/cwpump
  03-changeperm:
    command: chmod u+x /opt/cloudwatch/aws-scripts-mon/mon-put-instance-data.pl

option_settings:
  "aws:autoscaling:launchconfiguration" :
    IamInstanceProfile : "aws-elasticbeanstalk-ec2-role"
  "aws:elasticbeanstalk:customoption" :
    CloudWatchMetrics : "--mem-util --mem-used --mem-avail --disk-space-util --disk-space-used --disk-space-avail --disk-path=/ --auto-scaling"

- " Leader only" commands can be executed once during a deployment within the "container command" section. these are things like SQL scripts, which should only execute once per deploy (Not once by every instance)

- precedence order
- - Settings applied directly to the environment – Settings specified during a create environment or update environment operation on the Elastic Beanstalk API by any client, including the Elastic Beanstalk console, EB CLI, AWS CLI, and SDKs. The Elastic Beanstalk console and EB CLI also apply recommended values for some options that apply at this level unless overridden.
- - Saved Configurations – Settings for any options that are not applied directly to the environment are loaded from a saved configuration, if specified.
- - Configuration Files (.ebextensions) – Settings for any options that are not applied directly to the environment, and also not specified in a saved configuration, are loaded from configuration files in the .ebextensions folder at the root of the application source bundle.
Configuration files are executed in alphabetical order. For example, .ebextensions/01run.config is executed before .ebextensions/02do.config.
- - Default Values – If a configuration option has a default value, it only applies when the option is not set at any of the above levels.