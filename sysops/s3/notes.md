read this before the exam - S3 has many questions:
https://aws.amazon.com/s3/faqs/ 

Simple storage service

before exam reead the S3 FAQ

Standard S3, s3 glacier & s3 glacier deep archive
 - is designed for 99.99% availability (uptime) - but guarante 99.9
  - Amazon guarantee DURABILITY of 99.999999999% (remember 11 9's)
S3 IA is designed for 99.9 availability

5Tb file limit
unlimited storage
names must be unique globally
Example URL for bucket acloudguru
- - https://s3-eu-west-1.amazonaws.com/acloudguru
- objects have keys and values 
- - key is name of object, value is data inside the object
Can tag like any other resource

read after write consistency for PUTS of new objects (So for brand new file uploads)
- As soon as you PUT the file, you can read it.

Eventual Consistency for overwrite PUTS and DELETES (i.e. updating or deleting a file can take some time)
- Make a change in S3, but takes time to synchronise in the service

S3 IA (Infrequently accessed)
- cheaper at rest but higher charges for access
S3 one Zone IA (lower availability)
- reduced availability
- 20% cheaper
reduced redundancy storage (might not feature in the exam)
- has much lower durability

Glacier
- Very cheap
- Takes hours to get data back
Glacier deep archive
- - like glacier but 12 hours to get data back

Intelligent tiering
- S3 moved data to the correct tier based on it's access pattern
- uses frequent or in-frequent tier
- if not accessed for 30 days, it moved the data into infrequent access
- - then back again if it is accessed
- only fee for managing objects ($0.0025 per 1000 objects) 

Pay for
- storage by GB
- requeest (Get put copy)
- storage mgmt pricing
- - inventory, analytics
- data management pricing
- - i.e. data transferreed out of S3
- accelerate 
- - temporarily increase throughout to S3

Versioning
- use versioning to portect files if you accidentally delete them
- allows you to revert to an older version of a file

MFA delete
- Once enabled you need an MFA device code to PERMANEENTLY delete an objection version
- Once enabled you need an MFA device code to suspend or reactive versioning

WORM
write once read many - files which can be prevented from deletion for a set period of time.

server access logging
- logs access data about who is looking at S3 objects
- logs to an S3 bucket

cloudtrail
- can do object level tracking
- logs when anyone uses the S3 API

Bucket metrics
- See size of the bucket
- access patterns

life cycle rules:
- move data based on age to another tier (Or delete it)
- based on object CREATION date

Region replication
- replcate data top another region

Access points
- create your own access points for S3 access in your accounts

Set ACLs on S3 bucket OBJECTS
Set bucklet policies on S3 BUCKETS

Encryption (Serverside encryption)
SSE-S3 (encrypts data using S3s KMS key)
SSE-KMS (Uses your own KMS key)
- you can get logs as to when the key is used
SSE-C (Server side encryption with customer provided key)
- BYO key

client side encryption
- (encrypt the files ourselves and upload them encrypted)

If the file is encrypted at upload time you use the:
x-AMZ-server-side-encryption parameteer in the request header, there are 2 options for this:
- AES256 (S3 managed key)
- ams:kms (Custom KMS key)
This parameter tells S3 to encrypt the object at upload

You can create a bucket policy to deny any uploads without the parameter  "x-AMZ-server-side-encryption" forcing all uploads must be encrypted.
(This is STRINGNOTEQUALS condition in the bucket policy)

Use pre-signed URL with the CLI:
aws s3 presign s3://{bucketname}/filename.txt --expires 300

This creates a large URL for the file filename.txt which you can use to access the file without any creds.

- Usually done with the SDK
- Pre-signed URLs exist for a set amount of time (Default is one hour)
- Use --expires to expire links quicker

You can restrict S3 access by IP with the Condition statement:
"Condition":{
  "IpAddress":{"aws:SourceIp":"10.0.12.0/24"},
  "NotIpAddress":{"aws:SourceIp": "54.240.143.148/32"}
}

2 x Config rules exist which check S3 bucket permissions
S3 -public-write-prohibited
s3 -bucket-public-read-prohibited


With bucket policies keep in mind when ot use * and /* in policies. It is the difference bteween OBJECT and BUCKET level actions. BUCKET actions have BUCKET in the name. OBJECT level actions have OBJECT in the name.

If a policy has OBJECT level actions (i.e. put object, putobjectacl), then the bucket policy should have:
"Resource" : "arn:aws:s3:::examplebucket/*"

HOWEVER if the policy has bucket level actions i.e. listbucket or Deletebucket, the policy should have
"Resource" : "arn:aws:s3:::examplebucket"

Dynamic websites using server side scripting cannot run from S3.

Cross region replication can be done with prefixes for object names 
- i..e prod-*


You are an administrator with full admin access to S3. There are several S3 buckets within your organization that need to comply with a policy that requires all objects to be encrypted in-transit. What data encryption mechanism would you apply to fulfill this requirement?
- Use Client-Side Encryption.

Client-side encryption is the act of encrypting data before sending it to Amazon S3. To enable client-side encryption, use a master key you store within your application. Server-side encryption is encrypting data at rest. SSE-S3, SSE-KMS, and SSE-C are methods of server-side encryption and would not fulfill a data in-transit encryption policy. Protecting Data Using Client-Side Encryption