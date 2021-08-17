CloudWatch

EC2 monitoring by default is 5 min intervals
Detailed monitoring is 1 minute
Custom metrics the minimum granularity is 1 minute
Default EC2 metrics are:
-	CPU
-	DISK (IO only, not disk space usage)
-	NETWORK
-	STATUS CHECKS

to run cloudwatch with an on-prem instance you need the SSM agent installed too.

CloudWatch Logs
By default, keeps log data indefinitely
You can set custom retention dates though


To publish custom CW metrics you just need the IAM instance profile on the ec2 instance.

Dashboards can have metrics from all regions. You switch to a region add the metric then save it.
You just need to move to the region to see the metrics, then you add them to the dashboard.

Billing alarms in N-Virginia only (US-EAST-1)

for less than 1 minute (i.e. 1 second) you need to enable a custom metric (high resolution custom metric).


Metrics are retained for 15 months
- Data points < 60 seconds are availale for 3 hours (High resolution)
- Data points ~= 60 seconds (1 minute) available for 15 days
- Data points ~= 300 seconds (5 minutes) are available for 63 days
- Data points ~=3600 seconds (1hour) are available for 445 days (15 months)

If you need data longer you can pull it out using the api and then place it in your own data source.

Data is aggregated over time.
enable detailed monitoring for instances (1 min)
To aggregate service usage you need to enable detaield monitoring   

### Custom metrics ###
https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/publishingMetrics.html
Need IAM role on EC2 to push metrics into cloudwatch
aws cloudwatch put-metric-data --metric-name randomNumber --namespace Random --value 1000 --region ap-southeast-2

If you need to tweak the cloudwatch config file look in:
/opt/aws/amazon-cloudwatch-agent/etc/common-config.toml
(This is for proxy or credential info)

If you want to specify what logs the CW agent should send to CW put a .config file here:
/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.d
(See local cloudwatchagent.config)

/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:cloudwatchagent.config -s
(Configure the CW agent to start now and at reboot)

"Cloudwatchagentserverpolicy" IAM polocy exists for sending logs into CW