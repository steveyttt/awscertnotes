supports
- ```restful APIs``` (Stateless, serverless workloads)
- ```Websocket APIs``` (Chat apps)

restful APIs (representational State Transfer)
- optimized for serverless
- stateless
- supports JSON (Java script object notation)
- Uses Key/Value

supports ```multiple versions of APIs```
supports ```multiple endpoints and targets``` (each api endpoint can hit a different target)

supports ```throttling```
integrates with ```CW```

https://github.com/ACloudGuru-Resources/course-aws-certified-developer-associate/tree/main/Serverless_Webite_Demo

### API import ###
You can ```import your own APIs into API gateway```. This is done with an API definition file
- ```it supports open API (formerly swagger)```
- openAPI definition file can ```CREATE or UPDATE``` an exiting api
- Example of a file is here - https://swagger.io/docs/specification/basic-structure/

### SOAP (Simple Object Access Protocol) ###
- returns responses in ```XML``` (Came out in 1990)
- Can configure API gateway as a SOAP passthrough if necessary (https://rubix.nl/how-configure-amazon-api-gateway-soap-webservice-passthrough-minutes/)
- API gateway can ```transform XML into JSON```

### Supports caching ###
- Set it on a TTL (in second)
- Default is 300 seconds (5mins)
- improves performance and latency (Prevents having to call the backend everytime)
- To get none cached responses you need to
- - Include the ```Cache-Control: max-age=0``` HTTP header
- - - AND
- - Sign the request with a user or role that has the required ```execute-api:InvalidateCache``` permissions

### Supports throttling ###
- by default limits to 10,000 requests per second (Can be increased)
- maximum concurrent requests is 5000 requests across all apis per region (Can be increased)
- - So if you receive 10K requests in 1 millisecond API gateway will process 5000
- - Then it will THROTTLE and only allow another 5000 for the remainng number of milliseconds in that second
- If you go above these numbers you get a ```429 Too many requests``` error message   

To prevent your API from being overwhelmed by too many requests, Amazon API Gateway throttles requests to your API using the ```token bucket algorithm```, where a token counts for a request. You can enable ```usage plans``` to restrict client request submissions to within specified request rates and quotas. This restricts the overall request submissions so that they don't go significantly past the account-level throttling limits in a Region. ```Amazon API Gateway provides Per-client throttling limits that are applied to clients that use API keys associated with your usage policy as client identifier```.

### TODO ###
Look into stages in api gateway an roll backs

2 x types of authorizers for an API ```cognito``` and ```lambda```
- - Once configured they apply to ```methods``` in API GW

API gateway
- - to modify the front end interactions you should modify the configuration request / response 
- - to modify the back end interactions you should modify the integration request / response 
- In Api Gateway you call stage variables with 
- - http://exmaple.com/${stageVariable.<variable_name>}/prod
- - http://${stageVariable.<variable_name>}.exmaple.com/prod


ApiGW
- You can use stages for managing ```environments``` (dev, sit, UAT, PROD) and also ```VERSIONS``` of APIs, i.e. V2, V3
- Look into APIGW ```concepts``` - https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-basic-concept.html

- You can ```enable canary release``` for stage in api gateway. This allows for a % split of traffic
- You can also ```PROMOTE a canary release``` for a stage in api gateway. This pushes 100% of traffic to the new version.

- Usage plans throttle requests

- Request and response DATA MAPPINGS
- - Can be used for XML to JSON transforms
- - https://docs.aws.amazon.com/apigateway/latest/developerguide/request-response-data-mappings.html

- CORS response headers are, please note they need an ```options``` method too: 
- - Access-Control-Allow-Methods
- - Access-Control-Allow-Headers
- - Access-Control-Allow-Origin