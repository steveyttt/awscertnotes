compliance
- ISO
- - cert an org can get for documented information security management system
- PCI 12 controls
- - firewall
- - strong passwords
- - encrypt at rest
- - encrypt in transit
- - use AV
- - use AAA
- - restrict human and phsyical access to card holder data
- - track and monitor network access in card holder networks
- - perform pen tests
- - maintain a policy for info sec with all personel

- HIPAA (Health care information for US)
- FEDRAMP (Us government program)
- NIST (Cybersecurity risk framework)

- SAS70
- SOC
- FIPS140-2 (KMS meets level 2)

Inspector
- Scanning tool for Ec2 instances
- agent based
- needs a role
- scans nodes with specific tags
- provides a report of al checked completed (and pass / fail)
- DOES NOT SCAN RDS INSTANCES

- 4 packages (Rule packages)
- - CVEs (Common vulnerability exposures)
- - CIS
- - Security best practices
- - Runtime behaviour analysis

Rules have informational / low / medium / high criticality.

in shared responsibility model:
- Infra services (asgs, VPCs, EC2 etc)
- Container services (RDS, Lambda, EMR, Beanstalk)
- Abstracted Services (high level services) - S3, Dynamo, SQS, SES

SecGs are stateful. (If ingress is opened on a port then egress is allowed out too)

AWs artifact
- provides AWS compliance documents for auditors (Just get documents through it)

