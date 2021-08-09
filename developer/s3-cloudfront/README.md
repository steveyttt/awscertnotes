s3

Create S3 bucket policies easily: 
https://awspolicygen.s3.amazonaws.com/policygen.html

example index.html for static S3 website - https://github.com/ACloudGuru-Resources/Course-Certified-Solutions-Architect-Associate/blob/master/labs/creating-a-static-website-using-amazon-s3/index.html 

### S3 ###
- Enabling S3 Default encryption allows multiple types of encryption ```SSE-S3 && AES256 && SSE:C```
- -  If you need to be strict about the TYPE of encryption (S3, KMS, custom), you need to Add a bucket policy that denies PUT operations that don't contain the HTTP header ```x-amz-server-side-encryption: aws:kms```

acls are for objects
bucket policies are for buckets (DUH)

Public website buckets should have policies like this:
```
{
  "Version":"2012-10-17",
  "Statement":[{
     "Sid":"PublicReadGetObject",
     "Effect":"Allow",
     "Principal": "*",
     "Action":["s3:GetObject"],
     "Resource":["arn:aws:s3:::<MY_BUCKET>/*"]
  }]
}
```

Can only enable a ```custom``` key for encryption on an S3 bucket using SDK and CLI. 
Standard S3 encryption and KMS encryption can be done using the Console though.

CORS
Allow code from one resource to access another resource (i.e. S3 to another S3 bucket)
enabled at the ```URl level```. In the bucket hosting the source content you whitelist the other domain which will call you. i.e. HTML file is in bucketB.com, bucketA.com makes a call to B and a policy on bucketB allows it.

signed URL and signed cookies can be used to present additional or special content to paying users.

Can use custom SSL certs for cloudfront

use "invalidations" to remove old content from edge locations

Amazon S3 now provides increased performance to support at least 3,500 requests per second to add data and 5,500 requests per second to retrieve data, which can save significant processing time for no additional charge





### Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3) ### 

When you use Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3), each object is encrypted with a unique key. As an additional safeguard, it encrypts the key itself with a master key that it regularly rotates. Amazon S3 server-side encryption uses one of the strongest block ciphers available, 256-bit Advanced Encryption Standard (AES-256), to encrypt your data. For more information, see Protecting data using server-side encryption with Amazon S3-managed encryption keys (SSE-S3).

### Server-Side Encryption with Customer Master Keys (CMKs) Stored in AWS Key Management Service (SSE-KMS) ### 

Server-Side Encryption with Customer Master Keys (CMKs) Stored in AWS Key Management Service (SSE-KMS) is similar to SSE-S3, but with some additional benefits and charges for using this service. There are separate permissions for the use of a CMK that provides added protection against unauthorized access of your objects in Amazon S3. SSE-KMS also provides you with an audit trail that shows when your CMK was used and by whom. Additionally, you can create and manage customer managed CMKs or use AWS managed CMKs that are unique to you, your service, and your Region. For more information, see Protecting Data Using Server-Side Encryption with CMKs Stored in AWS Key Management Service (SSE-KMS).

### Server-Side Encryption with Customer-Provided Keys (SSE-C) ### 

With Server-Side Encryption with Customer-Provided Keys (SSE-C), you manage the encryption keys and Amazon S3 manages the encryption, as it writes to disks, and decryption, when you access your objects. For more information, see Protecting data using server-side encryption with customer-provided encryption keys (SSE-C).