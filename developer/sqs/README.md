### Lab Here ###
- https://github.com/JulieElkinsAWS/SQSLambdaTriggers

### SQS ###
- Can trigger ASG scaling based on tasks in an SQS queue
- pull / poll based system (EC2 nodes poll the queue for work)
- - push based system would be sns
- SQS = decoupling infrastructure (if you see decoupling then think SQS)
- Messages can be ```256KB``` in size (XML, plaintext or JSON)
- Get messages using the SQS API
- useful when work is producted faster than needed or nodes have intermittent network access
- guarantees the message will be processed atleast once
- messages can stay in the queue for 1 minute up to ```14 days```
- - ```default retention is 4 days```
- Failed messages can go into a ```dead letter queue``` once they have been attempted to be processed a number of times ```(between 1 and 1000)```
- Supports KMS encryption
- You can include customer metadata inside SQS messages by using ```message attributes```

2 x queue types
```standard```
- unlimited number of transactions per seconds 
- best effort ordering
- guarantee delivery at least once
- occasional duplicates of messages
or 
```FIFO```
- Exactly once processing
- No duplicates
- limit of 300 transactions per second (TPS)

```visibility timeout``` - This is the length of time a message is invisible when picked up by a worker. If the message is not fully processed by the time the ```visibility timeout``` occurs the message will become visible again in the queue and get processed by another node. 
 - Defaults to ```30 seconds```
 - maximum is ```12 hours```
 - API call to modify this is ```ChangeMessageVisibility```
 - - This can be done on a ```per message``` level and also a ```per queue``` level

```short polling```
- returns a response immediately, even if the sqs queue is empty
- still pay for these polls

```long polling```
- periodically poll queue
- The parameter to perform long polling is ```ReceiveMessageWaitTimeSeconds```
- doesnt return a response until a message lands in the queue
- can save money
- in most cases long polling is preferred
- - IF you need to save money or guarantee you receieve no empty requets, then use ```long polling```
- The maximum long polling time is ```20 seconds```

```delay queues```
- Specify a period of time which messages are invisble when they first enter an SQS queue
- - default is 0 seconds
- - maximum is ```900 seconds (15 minutes)```
- if enabled in a standard queue it applies to new messages
- if enabled in a FIFO queue it affects all new messages in the queue and all current messages in the queue

```large messages```
- messages over 256KB (Up to 2GB in size)
- These messages should be stored in S3
- Need ```Amazon SQS Extended Client Library For Java``` & ```AWS SDK for Java```
- - Cant use AWS CLI, CONSOLE< SQS API or any other AWS SDKs
