edge location - where content is cached
origin - origin of files (S3, EC2 instance, ELB or R53 address)
distribution - name given to the CDN

cdn exists to lower preessure on instances.

cache hit ratios:
- this is the number of requests served by EDGE locations (and not the source)
- So you want a high cache hit ratio as that will have better performance

you can view hits, missed and errors inside the Cloudfront console

maximise cache hit ratio by:
- specifying how long cloudfront caches objects (TTLs)
- - cache control max age sets how long data is cached at edge nodes before being updated again from source

- caching based on query string parametres (Case SENSITIVE)
- - www.example.com?/id=a1 www.example.com?/id=a2

- caching based on cookie values
- - separate cache behaviour based on cookies

- caching based on request headers
- - cache content at edge only based on specified headers (Avoid this if they have large unique values)

- Remove accept-heading encoder
- - this is related to gzip and compression

- service media content by http
- - deliver on demand live video streams

Cloudfront error codes:
- 400 (client side)
- - 403 permissions (S3 bucket with bad permissions)(Think get S3 object from Cloudfront to bucket)
- - 404 file not found, object not in S3

- 500 (server side)
- - 502 bad gateway (Cloudfront can't connect to origin)
- - 503 performance at origin
- - 504 gateway timeout before completing request