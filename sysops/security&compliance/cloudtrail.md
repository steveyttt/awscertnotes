CONTROL PLANE is the public API

- Can take up to 15 mins for an API call to make it into cloudtrail though
- enabled by default (7 day retention)

You can set this for one or all regions and one or all accounts.

What is logged?
- time
- caller
- request
- metadata
- response elements returned (success / deny)

Sent to an S3 bucket
- to delete it you need to use life cycle rules
- logs are sent every 5 mins to S3 buckets
- can aggregate across many regions and accounts
- You can encrypt logs
- You can integrate it with SNS
- You can protect the bucket with MFA delete and bucket policies
- Use S3 object lifecycle mgmt to move them to other tiers as needed.
- Bucket doesnt need encryption as default, but all files in the bucket will be encrypted.

You can enable log file validation. 
- This creates a HASH for every file in S3. This is known as a DIGEST file. 
- You use the digest file to check the HASH to check that a cloudtrail log has not been modified.
- Used for log file validation

Use IAM and S3 bucket policies to restrict access to CloudTrail logs.