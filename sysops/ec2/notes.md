Types of EC2
- on-demand

- reserved (Up to 75% off) (one or 3 year model)
- - There are standard and convertible RIs
- - There are scheduled RIs allowing you to run instances in set day / hour window

- Spot (Bid model) (quick shutdown model)
- spot block (request spot but for a specific duration) (maximum of 6 hours)

- Dedicated hosts
- - You get your own physical host and run EC2 instances on it
- - Good for licensing and regulatory
- - You can buy them as an Ri too

Launch Issues
- instancelimitexceded error (hit the limit on number of allowed instances)
- - accounts start with a limit of 20 (You then need to request a limit increase)

- InsufficientInstanceCapacityError
- - AWS does not have hardware capacity for this resource type in the AZ you chose
- - AZ specific error
- - WAIT if this happens, or use less instances, purchase Ris, change AZ, Change instance type

PLACEMENT GROUPS
Placement group - Place instances close together in the same AZ for 10Gbps connectivity (Fast connections, but everything runs in one AZ)
- low latency, high throughput

- 3 types
- - cluster - instances all in one AZ (LOW LATENCY, HIGH NETWORK)
- - - - Get 10Gb network
- - - - bad for availability

- - Partition - instances are created in partitions each in separate racks (THINK LARGE DISTRIBUTED WORKLOADS (HDFS, HBASE, CASSANDRA))
- - - - Here you are separating at the partition level (so you can have many nodes in one partition)
- - - - Partitions cannot share racks
- - - - Could have 2 partitions each with 2 instances in them, so you can do multiAZ

- - Spread - each instance is in a separate rack (GREAT FOR MAXIMUM RESILIENCE, NOT SO MUCH FOR HIGH THROUGHPUT, LOW LATENCY)
- - - - Great for workloads with a small number of instances. 
- - - - Limit of 7 running instances per AZ

autoscaling issues that can happen:
- associated key pair does not exist
- sec g does not exist
- asg config is not working correctly
- asg not found
- instance type not supported in az
- az no longer supported
- invalid EBS mapping
- autoscaling service not enabled in account
- attempting to attach EBS to instance store ami

volume typees
- ebs is persistent
- - delete on terminate is DEFAULT
- - ebs root volume can be 1 or 2TB depending on OS
- - is deleted by default when EC2 instance is deleted 
- - - This can be prevented by unticking the "delete on termination" option
- - None ROOT EBS volumes are NOT deleted when the instance is deleted
- - - These need to be deleted manually

- Instance store is not persistent (Ephemeral)
- - you cannot stop an instance using instance store only DELETE
- - - Instancees can only be RUNNING or TERMINATED
- - Keep in mind that it should only be used for data you can lose
- - instance store is max 10Gb
- - always deleted when instance is deleted
- - Non-Root instance store volumes are deleted too.

root volume is where OS is installed.
- can be EBS or instance store

AMIs
- AMIs are regional
- Needs copying and registering per region
- you cannot directly copy an encrypted AMI shared by another account.
    - - - UNLESS you copy the underlying snapshot using your own KEY and create a new AMI from that snapshot.
- you cannot copy an AMI with an associated billingProducts code (i.e. market place) (i.e. redhat, windows server)

Encrypted AMIs
You are trying to copy a custom AMI which has been shared by another account. The AMI has been encrypted. What steps will you need to take to successfully copy the AMI?
- The sharing account must share the underlying EBS snapshot as well as the original encryption key used to encrypt it. Copy the EBS snapshot and re-encrypt it using your own key, then register it as an AMI


HVM is the preferred instance type. Para virtual is a different type that used to be faster, but HVM has caught up now.
- Linux can be HVM or paravirtual
- Windows can only be HVM

Paravirtual uses the host hardware a little more, so needs more host access.

Only AWS Staff administrators have access to hypervisor hosts

All storage memory and RAM is scrubbed before it is delivered.

Dedicated instances:
- Instances which run on dedicated hardware (Think like you can lock your EC2 instances to a certain physical server)
- pay on-demand, use spot or RIs
- Useful with licensing issues (Hosts based licensing)(Usually windows)
- may share hardware with other instances from the same account.

Dedicated hosts:
- Physical hosts you can provision instances on
- charged by host (AKA Physical server)
- Useful for licensing issues
- visibility into Cores / sockets / host ID


Autoscaling groups can have scheduled times to provision and reduce the number of running nodes (it's supported in Cf too)

Launch Templates
- Existing Launch templates CANNOT be updated, but new versions can be created
- Changing an ASGs launch template doesnt affect runnin ginstances
- you can only select one launch template for an ec2 ASG at a time

Launch COnfigurations are being deprectaed and launch templates are the next thing
- spinning up on-demadn and spot instances does not work in launch configurations, only launch templates.