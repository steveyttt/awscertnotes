5tb file limit
https://bucketname.region.amazon.com

ACls apply to objects

versioning
works with lifecycle rules

Glacier - hour to minute retrieval

glacier deep archive - over 12 hours to get back

worm = s3 object lock (can be done per object or bucket)
governance - human can overwrite
compliance - no one can over write (not even root)

S3 has object lock (WORM) for glacier called S3 glacier vault lock.

SSE - server side encryption
- sse-s3 (AES)
- SSE-KMS (Bear in mind KMS transaction limits)
- SSE-C

Client side encryption - you encrypt and upload


3500 capacity per prefix
5000 gets per prefix 

s3 byte range fetches (get smaller blocks of files)

multipart must be used for 5GB +

objects in an existing S3 bucket are mpt replicated when it is enabled.




ec2 enhanced networking > 10Gbps to 100Gbps

EFA - HPC / machine learning

hibernate works on C,M,R families.
preserves mem in ram
ram must be less taht 150GB
win / lin / lin2

FSX for lustre - HPC compute linux

dynamo db transaction??? Think ACID

dynamo does point in time recovery. Not enabled by default though..

streams must be turned on for dynamo db global tables

multicast works on TGW

VPN HUB - simplify VPN topology

R53:
diff between alias and cname. always choose alias.
R53 can do record health checks

ELB deregestration delay - keep connections open on instnces after they have been removed / disabled in the ELB.

dynamo:
spiky workload - use on demand scaling
predicatbke workload spikes, use Dynamo auto scaling

SQS - check developer notes
maximum 14 day retention of messages

redshift is not highly available. Only works in one AZ!!

EMR uses EC2 (You can use spot with it)

EMR excels at extract, transform, and load (ETL) jobs

Redshift can hold up to 16 Petabytes of data per cluster

Security
Shield protects against L3 and L4 attacks only (DDOS too)
- Network
- Transport

Shield advanced is 3K a month

WAF
- Allow all with exception
- Block all with exception
- Block XSS, L7 attacks, SQL injection

GD
- monitors CloudTrail, VPC FLow Logs, DNS Logs
- findings create events to trigger lambda

MACIE
- Analyze S3 data for PII / Financial data / PHI
- integrates with event bridge

Inspector
- performs host or network assessments

KMS
- Create a CMK, then use that for all other keys
- Can import your own keys
- Can use Cloud HSM
- Keys can be assigned key policy for who can use them
- - Can also be done with IAM policies

HSM is full hardware you get access too
KMS is managed by AWS (rotations, generation, but with shared tenancy)

Secrets Manager
- Used for DB creds, API keys, SSk keys, password
- auto rotates passwords (Secrets manager does this by default)

Parameter store versus secret manager
- param store is cheaper, limited to 10K, does NOT perform key rotation and cannot dynamically generate passwords using CF.

IAM > Deny trumps an allow

AWs Certificate manager - works with ELB, CF and API GW. Seamless integration and auto rotates certs.

Automation
For SSL on a static S3 bucket you need CloudFront

Caching
Global accelerator == IP caching
In-memory database (redis or Dynamo)
- - usually favour DynamoDB
Redis has more features than MemCacheD.

RDS
From an RDS instance you can create a read replica in the SAME or DIFFERENt region.

AWS data sync
migrate data from on prem to S3, FSX, EFS
Can also perform AWS to AWS migrations between the above services.

EFS
can do data verification (checks file,s but slows down the process)

CloudFront
Restricting access to Amazon S3 content by using an origin access identity (OAI)
- Create a special CloudFront user called an origin access identity (OAI) and associate it with your distribution.
- Configure your S3 bucket permissions so that CloudFront can use the OAI to access the files in your bucket and serve them to your users. Make sure that users can’t use a direct URL to the S3 bucket to access a file there.
