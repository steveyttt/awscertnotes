gp2 for up to 16000 IOPS (SSD) (10,000 was the old amount)
io1 (provisioned IOPS) 16000+ and 160MiB+ IOPS (SSD)
- goes up to 64,000 IOPS

st1 Throughput optimized (HDD) - HDD can never be boot volume
sc1 cold (HDD) - HDD can never be boot volume
spinning disk (Cheap and lots of it) use st1 or sc1 (Sc is the worst performing).

GP2
- base of 3 IOPS per GB
- Max size of 16384 GB (16TB)
- Maximum IOPS size of 16,000 IOPS

- In this logic a 10GB disk would get 30 IOPS
- HOWEVER we can BURST our IO credits up to 3000 IOPS
- Volumes get an intial credit balance of 5,400,000 i/o credits (This can sustain 3000 IOPS a second for 30 minutes)

After 16,000 IOPS you neeed IO1
If you need 16000 IOPS or more use provisioned IOPS (IO)

If you need more than 3000 IOPS consistently then just provision a bigger volume of GP2.

If you restore an volume from snapshot the data needs to be pulled from S3. This can make the disk slow initially. If you "initialize" the disk before use (effectively read every block) first then the performance will be OK.
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-initialize.html

For exam keep in mind these CW metrics 
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-volume-status.html
- Volume read OPS 
- Volume write OPS
- Volume queue length (disk operations queued)
- VolumeThroughputPercentage (% of throughout used)

Volumes have status checks too
- ok
- warning (operational but slow)
- impaired (broken, will be causing issues)
- insufficient data

You can dynamically change the IOPS performance, size and volume type of a volume.

If you hit your IO limit you with GP2 you can incresse the volume size (To make it faster).
- IO is capped at 5.2TB EBS size though (as thats 16,000 IO)
- You can provision the disk to 16TB, but the allowed IO is maxed at 5.2TB
- Change storage class to IO1 to break 16,000 IOPS

VOLUMES are in the same AZ as the running instance.
- You can change volume type and size on the fly

SNAPSHOTS are regional based though
- STOP your instance before performing a snapshot (as a best practice)
- When restoring a snapshot you can change the volume type (GP2, ST, IO1 etc)
- You can copy a snapshot from one region to another
- You can create an AMI from a snapshot
- Snapshots exist in S3 (You can't see them, but they are there)
- - snapshots are incremental (so 3 snapshots of a 5Gb instance wont be 15Gb, more like 6GB) - Think Incremental backup types (Just keeps diffs)
- first snapshots take a long time
- snapshots of encrypted volumes are encrypted automatically
- - volumes restored from encrypted snapshots are encrypted automcatically
- You can share snapshots - But only if they are unencrypted
- Snapshots affect disk performance when they are taken (ESPECIALLY ON SPINNING HDD ST1 & SC1)
- You can create snapshots from instances for which hibernation is disabled.

Snapshot lifecycle manager can create snapshot lifecycle policies to create and delete snapshots of EBS volumes
https://aws.amazon.com/blogs/aws/new-lifecycle-management-for-amazon-ebs-snapshots/


When Amazon EBS determines a volume is inconsistent it disables I/O, this causes the volume check to fail.To access the volume fater then you should
- SWITCH ON Enable VOlume IO