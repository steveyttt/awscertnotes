Most resources only allow encryption at ```CREATION```

EFS, EBS & RDS both need creating new to use KMS.

EBS volume - encryption must be enabled at creation time. You cant encrypt an unencrypted disk and vice versa.
- You can create a new volume though and RSYNC filees between the 2.

S3 buckets and objects - can enable encryption at any time.

KMS
- Multi tenant cryptographic keys
- symmetric
- You can perform the following actions
- - import your own keys 
- - disable and re-enable keys  
- - define key management roles in IAM

Cloud HSM - DEDICATED HARDWARE
- asymmetric
- generate your own keys etc
- resides inside a VPC
- FIPS 140-2 level 3 compliance
- suitable for contractual or regulatory requirements

symmetric
- use the same key to encrypt and deecrypt data

asymmetric
- different key or algorithm for encryption or decryption actions

Penetration testing
- You can now pen test your own EC2 instances without authorization from AWS

AWS certificate manager allows you to provision, manage and deploy certs.
- Can manage new and running certs
- Can use external certificates

If ACM is not supported in a region you can upload certificates into IAM. BUT the recommended best practice is to provision new OR import your existing certs into ACM (Amaon Certificate Manager).
