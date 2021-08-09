read this - https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf (Don't have to read for the exam)

Amplification / Reflection
- query an NTP server and put the source IP as a victims IP. Then, when the NTP server responds to the query it send traffic to the victims IP.

L7 attack
- smash an instance with many requests

slow loris attack
- Sends many connection requests SLOWLY, this keeps threads open on boxes for a long time and starving the connection pool.

AWS shield (Free)
- protects all AWS customers on ELBs, cloudfront and R53
- protects against SYN/UDP reflection and other L3/L4 attacks

AWS Shield advanced (3K per month)
- protects ELB, Cloudfront and R53
- Always on flow based network traffic
- DDOS response team
- protects bill against higher bills

Penetration testing needs approving first.
https://aws.amazon.com/security/penetration-testing/ 


Web App FW
- Protects API gateway
- ALB (NOT CLB or NLB)
- Cloudfront

WAF
- allow
- block
- count

What can it protect against?
- Ip addresses
- Countries
- values in request headers
- strings that appear in requests (regex patterns)
- length of request
- presence of SQL code (SQL injection)
- Presence of a script (XSS)

