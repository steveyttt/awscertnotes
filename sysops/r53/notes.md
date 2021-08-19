Alias records are used for direct mapping to AWS resources. They can be apex records (DNS zone root names). ```CNAMES CANNOT BE apex records```
- ELBs
- Cloudfront
- S3 buckets
- beanstalks
- API gateways
- global accelerator
- VPC endpoint

ALWAYS choose ```ALIAS``` over CNAME.

R53 does not support TLSA records.(method to finger print and certs for sites)
SOA - Start of Authority. Authority information for the domain.

### Simple Routing Policy ###
many IPs under one DNS record. R53 will choose the destination IP randonly with little logic. (No health checks)

### Weighted Round Robin ###
Specify that xx% of requests go to one target and xx% goes to another target.
Done using the Weight option. The higher the weight the more traffic it will receieve.
You create multiple individual Weighted round robin records using the same DNS name but pointing to a different destination IP.

### Latency Routing ###
Route traffic based on closest latency
create many DNS records for the same name, for each record specify the destination IP and the region it resides in.

### Failover Routing ###
Needs a health check to work, health checks work with:
- endpoint polling (Port, name and URI path)
- status of other health checks
- state of cloudwatch alarms
Once you have a health check you create two (Or more) DNS records with the same name but different endpoints. Then you set one DNS record as primary and another as secondary. In the event the health check has issues the records will switch.

### Geolocation routing ###
Route customers based on their location to specific regions / endpoints.
Uses source LOCATION (Does not use latency)
i.e. europenas to London region
i.e. Americans to IAD region.

### Geo Proximity routing ###
Use traffic flow rules (New?)
regional based routing based on source
allows you to visually look at a map of the earth and see what locations are covered by what content
Can use bias configuration to make some areas cover a bigger geography

### Multivalue answers ###
Similar to simple routing policy except you create one DNS record per destination IP. You can also add a health check to multivalue DNS records, so in the event one IP goes offline R53 will immediately stop sending traffic to that destination.

REad through atleast once:
https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html