Amazon Certificate Manager
Easily provision and manage public / private TLS certificates
- provides central mgmt
- use is audited in Cloudtrail
- Has private CA authority
- integrates with AWS services seamlessly (ALB / CloudFront / API Gateway)
- Can import certificates from other certificate providers
- - Can use DNS or E-mail to validate domain ownership to get a certificate
- Can still import certs into IAM, but it is preferred to use ACM instead