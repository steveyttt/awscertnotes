AWS-RunShellScript (Run a one off command / script on an EC2)
- commands can be outputted to S3
- executions can create SNS notifications

- Need SSM agent installed to run on-premises instances.

Integrates with cloudwatrch dashhboard
group resources by Tags and resource groups

You need to enable systems manager advanced-instances tier when:
- Greater than 1000 nodes (Per account, per region)
OR
- Windows nodes need patching too
