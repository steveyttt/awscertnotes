metrics they provide
- unhealthy host count
- healthy host count
- latency
- request count
- http 5xx
- elb 5xx
- http 4xx
- backend connection errors
- 200s
- 400s
- surge queue length (Number of pending requests - maximum of 1024, after that the request is dropped)
- spill over length (dropped connections over 1024 surge queue length)
- - Classic load balancer only for spill over and surge queue length

Can enable logging and put it in s3, they are uploaded on a 5 minute or 60 minute interval and are compressed. logs contain:
- Time request was received
- client IP
- latency
- request path
- server response

You are never provided the IP of an ALB or CLB, just the DNS name

ELB metrics are gathered every 60 seconds.

Keep in mind LEB logs in S3 are the only way to store persistent log information to perform historical checks.

Request tracing can be done on ALBs ONLY to track http requests from clients to targets to servers. the ALB will add and update the X-Amzn-Trace-Id header before sending the request to the target.

ALB - HTTP & HTTPS - Layer 7 (request routing based on path / domain etc)
NLB - TCP - Layer 4 - Extreme performance- millions of requests per second - Ultra low latency - Most costly
- Creates a static IP for LB in each AZ so you can easily configure firewall rules as needed for managing access
- Other LBs have IPs which change
Classic (CLB) - Layer 4&7 - Sticky sessions - X-forwarded-for (not recommended anymore)

Pre-warming
- create a support ticket and ask AWS to warm your LB
- They need request rate p/s and timeline of how long you need it warmed
- They need standard packet size
- ELb performance Will gradually increase in size based on demand, but for spiky loads you should warm

Error messages
4xx client issue
- 400 malformed request
- 401 user access denied
- 403 forbidden
- 460 client closed connection before ELB responded
- 463 load balancer has received an X-forwadrd for with more than 30 IPs

5xx server side error message
- 500 internal server error
- 502 bad gateway (app server closed request to LB)
- 503 service unavailable (servers are down - no registered targets)
- 504 gateway timeout (Application not responding App, cache, DB)
- 561 unauthorized (i.e. if you are using an IDP)

