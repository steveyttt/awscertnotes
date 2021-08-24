Secrets Manager:
- Encrypt keys at rest using your own KMS keys
- Ise cli / API / SDK or console
- Replace hard coded credentials with api calls to secrets manager
- Secrets can be rotated automatically on a schedule
- Secrets are classed as "RDS", "none RDS data bases" or other
- Automated rotation of secrets can be done with AWS services
- - It creates a lambda function to do this for you
- You are provided sample code when creating a secret which demonstrates how to access the secret