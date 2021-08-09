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

