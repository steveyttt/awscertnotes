Smallest VPC is a /28 subnet (16 IPs) - Then minus 5 consumed by AWS so only 11 useable IPs.
largest VPC is /16
Only one IGW per VPC & IGW fee is by traffic processed (No hourly cost)
No transitive peering via a VPC peer

You can VPC peer across regions.
- as usual CIDR ranges must be unique
- May not be available in EVERY region yet

5 IPs are always consumed in a subnet
.1 is VPC router
.2 is DNS
.3 is for future use
.255 is broadcast
.0 is network address

You can enable "auto assign public IPV4 address" at the Subnet level in the VPC settings.

If you use a NAT instance (Old school) you need oto remember to disable the "Source/Destination" check feature.
- basically any instance which is not the sourve or destination in the network path (Any network appliance) should have this enabled.
- instance size will determine how much traffic they can run
- They can go inside an ASG if needed

Need a NAT gateway per AZ - supports up to 10Gb network burst.
- No secG needed on NAT GW
- Auto assigned a public IP
- Need to update route tables.

NACLs
- Allows configureable inbound and outbound rules
- One subnet can only be assigned to one NACL
- rules are numbered in the NACL - increment by 100 at a time
- NACLs are created within one VPC and then assigned to that VPCs subnets
- NACLs when created new always DENY EVERYTHING
- Need to open ephemeral ports (client side ports) OUTBOUND to respond to requests
    - PORTS 1024 - 65535
- support in-bound and outbound rules.
- NACL rules are assessed in numerical order
 - - So if you have NACL rule 1 as - approve 0.0.0.0/0 on port 80
 - - Then a NACL rule 2 as -  deny for 10.0.0.0/32 on port 80
 - - Then the deny will NEVER kick in, as everything will be approved by the first rule

 VPC Endpoints
 2 types of VPC endpoints (interface and gateway)
 - Gateway endpoints exist for S3 and Dynamo. 
 - - and can be used with route entris in the route table.
 - Everything else uses an interface endpoint (Which is a basic ENI under the hood)

 VPC flow logs
 created at the VPC, Subnet and Network interface level
 Once you have created a flow log you cannot change the config (IAM permission, destination CW log group) (To do this you will need to recreate the flow) 
 You can't enable flow logs on a peering VPC which is not in your account(s).
 Some traffic is not monitored in flow logs
 - Traffic using AWS DNS IPs
 - traffic fo windows activation
 - traffic to 169.254
 - traffic to the reserved IP for the VPC default router (.1)
 - DHCP requests are not logged

 cidr.xyz.com

 DXgateway now allows you to use your DX physical link to connect to multiple regions (i.e. US WEST1 and US EAST1 connectivity from one DX link)

 All accounts must be part of the same master payer to do multi account DX support. One DX Gateway can handle many accounts, you do not need a DX gateway for every account.

