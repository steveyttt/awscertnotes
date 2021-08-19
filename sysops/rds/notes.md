Mysql, maria, mSSQL, postgres, oracle, aurora

elasticity - short term scale up - scale with demand
scalability - scale out infra long term

### RDS ###
multi AZ - keeps DB copy in another AZ for fast failover
- when one AZ fails AWS updates the DNS for the RDS instance to point to the instance in the other AZ
- - amazon handle the DNS failover
- This is for availability, not performance (Not a scaling solution)
- RDS keeps sync between 2 copies
- - use synchronous physical (postgres, mysql)
- - in ms sql server uses a differenet type
- backups and restores are taken from the secondary copy once multi az is enabled
- - this lowers the burden of thesee operations on the master RDS instance
- You can manually force an RDS failover in the console or with the api
- enabling on a single instance causes an IO freeze
- When you reboot an RDS instance with Multi AZ you can choose "reboot with failover", i.e. promote other node first then reboot.
- - this is a good way to simulate a failover.

### Read Replicas ###
read repliacs are RO replicas of an RDS instance
- used for performance scaling
- only RO version, so only supports RO actions
- uses the engines native async technology
- provide a different DNS link for RR
- can promote a RR to be a primary (But it will briefly break synchronisation)
- You cant create a RR until you enable backups!

- good for busines reporting or DW solutions
- - may come up in exam that prod RW db is suffeering performace. How can datawarehouse team lower performance? EASY create a Read Replica and point datawarehouse at that instance. OR OR OR use redshift

### When creating Read replicas ###
- enabling RR will cause a brief suspension of IO to the master instance UNLESS you have multi AZ enabled. Then it will use the standby RDS instance. (So use multi AZ)
- Maximum of 5 RR for postgres, myssql and maria
- can have RR in different regions for all engines
- replication is async not SYNC
- RR can be multi AZ!!!!
- You can have RR of RR (Big latency issues here though)
- You cant take snapshots of RR (But you can enable automatic backups)
- key metric is REPLICA LAG
- Know the difference between RR and MultiAZ

### Async replication ###
Mysql
postgres
Maria DB

### backups ###
You can set a maintenance window (This is when auto upgrades can happen)
You can set backup schedule and retention period
you can enable termination protection
enabling backups on a running instance causes a reboot
- You cant create a RR until you enable backups!

### instances can be encrypted (RR too)
enhanced monitoring gets metrics every 60 seconds


### Steps to encrypt an instance (Which was created without encryption)
- take a snap of an RDS instance
- copy snap to same or different region
- then encrypt during the copy
- restore the snap in RDS

### sharing encrypted snapshots between accounts (You can do it - JUST NEED CUSTOM KMS KEY)
- FIRST you need a custom KMS keys (This is because you can only share CUSTOM KMS keys between accounts)
- create a snapshot using the kms key
- share the custom KMS key to the accounts
- then share the RDS snapshot with the other account

- - You cant share encrypted snapshots PUBLICLY
- - You cant share oracle or MSSQL snapshots encrypted with TDE
- - you cant share an RDS snapshot encrypted with a default KMS key

RDS Versions - CHECK IN CONSOLE OR WITH CLI
aws rds desribe-db-instances --region $REGION       

### AURORA ###
MySQl and PostGres compatible
3 x better performance than postgres
5 x better performance than mysql
RDS instance does not need to be regional but can be global!
Can be publicly accessible like any other RDS instance
Encryption at rest is enabled by default - this means all RR are encrypted too
starts with 10GB of storage and scales disk automatically
64vCPU 488GB of memory maximum
2 copies of your data is stored in 3 AZs so up to 6 versions in one region!

Must be created in a VPC!
Encrypt with KMS
You cant encrypt and unencrypted database (It needs to be rebuilt)

### Connection Management ###
Cluster Endpoint - connects to primary DB (For writes)
Reader Endpoint - provides a load balanced endpoint for RO activities
Custom Endpoints - configure an endpoint which targets a specific aurora instance (like one specific to a team / application)
instance endpoint - endpoint for a specific auroa instance

### Aurora backups ###
35 day backup retention period.
point in time restore up to a second
no impact to DB performance
Stored in S3
Can take snapshots and store them in S3

Storage is self healing - data blocks and disks are scanned constantly and corrected if errors are found  

2 replicas available
- auroroa replicas (currently up to 15)
- - low performance impact, replica can be a failover target with no data loss
- mysql read replcais (currently up to 15)
- - higher performance impact on DB
- - Minutes of data loss at failover

if cpu is 100% - - EXAM TIP
- if this happens and writes are causing an issue you need to scale UP
- if this happens and reads are causing an issue you scale out to RRs

auroroa serverless
power on off and scalse based on demand (pay oer second)
automatically scales compute based upon demand 

When aurora fails over to read repliaces you can get it to choose which RR to failover too based on TIERS. EXAM TIP
- so for example one RR could be tier 0 priority and another RR could be tier 15.
- In the above scenario the instance will failover to the RR that is tier 0 (That is highest priority)

You can create CROSS REGION REPLICAS with aurora.
- Only do this if you have MultiAZ enabled though on your instance.
- This creates a whole new Aurora cluster in the target replica region (If replication is disrupted you need to recreate the target region aurora cluster)
- If you do not you may need to perform manual configuration in the cross region replica zone if th primary region has a problem

Cannot delete aurora at a cluster level with running instances, you need to delete by node (instance level).

TODO
Create a list of all RDS common ports
3306 for MySQL
1433 for MSSQL
5432 PostGres

MSSQL - Cannot scale storage live!