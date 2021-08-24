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