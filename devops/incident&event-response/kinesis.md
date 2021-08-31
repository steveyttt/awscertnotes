### kinesis ###
- kinesis streams (Stream data or video)
- kinesis data firehose
- - capture transform and load data into AWS data stores for real time analytics
- - Think about when we used lambda in firehose to manipulate CW logs and push thenm to another location
- Kinesis data analytics
- - interactively run SQL against streaming data and store output in an AWS data store

There is a ```kpl``` kinesis producer library:
https://docs.aws.amazon.com/streams/latest/dev/developing-producers-with-kpl.html

### Kinesis streams ###
- retains data for 24 hours by default
- maximum of 7 days
- data is kept in shards
- Producers produce data and then consumers consume the data (Well duh)
- - push data in, grab the data, do something with it, archive the data where you need it
- ```shards``` ONLY relate to kinesis streams (Not other kinesis features)
- - More shards = more performance
- - 5 reads per second
- - 2MB read rate per second
- - 1000 writes per second
- - 1MB per second
- - - Each shard can process one stream of message(s)
- - - If you have 4 shards processing messages and one EC2 instance consuming messages from the streams it will be connecting to all 4 shared ```HOWEVER``` If you have two EC2 instances consuming messages, each instance will consume 2 shards each.
- - - - ```important``` with the Kinesus CLient Library ensure the number of instances does not exceed the number of shards
- - - - You NEVER need multiple instances to handle one shard
- - - - One worker can handle many shards though
- - - - The Kinesis CLient Library (KCL) handles the balance between processing instances and shards. If you dynamically increase the # of shards the KCL will dynamically reconfigure the nodes to consume the additional shards
- - - - CPU utilisation is what should drive the qantity of consumer instances you have (NOT the number of shards)
- Massiely scalable (GBs per second)

### Kinesis video streams ###
- If it says videos in the question then think ```kinesis video streams!!!`
- Works with millions of devices
- Integrates with amazon recognition
- Build real time video applications (like traffic light bots)

### Kinesis Firehose ###
- No shards
- No consumers of data
- infinite retention
- lambda can be used as part of the kinesis firehose configuration to transform the data.
- Accepts up to 1000KB
- Can save direct into S3, Redshift, Splunk or Elasticsearch if needed

### Kinesis Data Analytics ###
- Still have producers
- Run SQL queries on data as it comes in
- sql output can go into Redshift, S3 or ElasticSearch
- Works with STREAMS and firehose
- IF you see ```REAL-TIME ANALYTICS``` then think KINESIS DATA ANALYTICS

### General ###
- Can use KMS encryption on resources
- Can enable Shard level metrics (There is an additional cost for this though)

### Hints ###
- If you see ```realtime or streaming``` in a question, think KINESIS!
- https://docs.aws.amazon.com/streams/latest/dev/kinesis-record-processor-scaling.html

1000's of logs with realtime analysis = Kinesis
Single log files with periodic analysis = cloud watch logs

### Exam Tips ###
