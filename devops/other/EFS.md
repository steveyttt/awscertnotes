Look inside Sys Ops notes

Can mount EFS from on-premises (Over DX and VPN)

There are two options for the service: 
- General Purpose (Default, good for latency sensitive, content mgmt, home dirs, websites)
- - or 
- Max I/O (large scale, higher latency, big data, parallel apps )

There is standard and EFS IA

works cross VPC and cross AZ and Cross datacenter!

Uses NFS

Multiple EC2 instanmces can access the files at once.

Has a built in life cycle management (Can move unused files to infrequent access tier (Like S3]))
- EFS infreqyent access is different to S3 infrequent access (Same logic, but different storage)

Supports encryption at rest and in transit
- At rest you can choose a KMS key
- in transit you don't seem to provision a certificate, just specify TLS in the cmd line

Can only enable encryption AT CREATION!!!!

EFS for linux and FSX for windows

You create a mount target per AZ for your EFS storage. (For redundancuy)

Can choose between bursting and throughput modes (not too important for exam though)
- bursting scales storage and throughut together
- throughput is a set IO you specify

Uses security groups for connectivity

EFS Does not have a native backup functionality.
- To backup EFS use an Ec2 instance with the AWS DataSync agent.