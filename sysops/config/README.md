AWS config stores everything in an S3 bucket.(local account or remote)
Config is ```regional```.
Can hook up to SNS.
You should only enable checks for global resources in one region. (I.E. IAM)

Config Need Read Only IAM permissions to the account it runs in (Usually a managed role)
- Need SNS permissions
- Needs write access to S3 bucket

You choose how many config rules you want to enable.

Rules are triggered on schedule or by event

Use config recorder for the timeline of when things change in resources.
For who provisioned resources look in CLoudTrail.

You should set IAM permissions for which admins can use config

If config is disabled by someone the api call to disable it will be logged in cloudtrail.

You can turn recording off from the console. To delete the recorder you need to use the cli
```aws configservice delete-configuration-recorder --configuration-recorder-name default```