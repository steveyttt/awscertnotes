monitors:
- unusal API calls
- unauthorized deployments
- compromised instances (Bit coin miners)

uses:
Threat intelligence feeds
Machine learning
CW events
Cloudtrail events
R53 DNS
VPC flow logs

Enable it at the account and region level.
integrates with logging / alerting / lambda to perform actions when problems occur

Has an IAM service role
reports findings every 5 minutes
Integrates with CW events
Has a concept of master GD account which all accounts subscribe too.
Integrates with organisations now

Priced based on:
- VPC flow logs and DNS logs ingested
- CLoud Trail analysis

You can suspend GD (Keep data but stop its use)
you can disable GD (Turn it off and wipe data)