Needs BOTH agent and sdk to work
install the agent
instrument the SDK with your app code, it then sends data to the locally running xray daemon on the node 
In ECS install the XRAY Daemon in its own Docker container on your ECS cluster alongside your app

- xray sdk then gathers request / response headers, code info, metadata, latency data, http requests, error code.

Provdes a servicemap (visualization) of how workloads connect to one another.

TEST THIS WEB APP
https://console.aws.amazon.com/elasticbeanstalk/home?region=us-east-1#/newApplication?applicationName=scorekeep&solutionStackName=Java

annotations
- record additional info about requests into XRAY
- Key/Value pairs which can be grouped and searched
- - example would be in gaming setting an annotation of gamename or game ID

https://docs.aws.amazon.com/xray/latest/devguide/xray-api-segmentdocuments.html
- A trace segment is a JSON representation of a request that your application serves. A trace segment records information about the original request, information about the work that your application does locally, and subsegments with information about downstream calls that your application makes to AWS resources, HTTP APIs, and SQL databases.
- A segment document conveys information about a segment to X-Ray. A segment document can be up to 64 kB and contain a whole segment with subsegments, a fragment of a segment that indicates that a request is in progress, or a single subsegment that is sent separately. You can send segment documents directly to X-Ray by using the PutTraceSegments API.