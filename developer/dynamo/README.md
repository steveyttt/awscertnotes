### to do ###
- Understand the dynamo partition key and search index logic much better
- - read this - https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/#:~:text=DynamoDB%20uses%20the%20partition%20key's,which%20the%20item%20is%20stored.&text=All%20items%20with%20the%20same,by%20the%20sort%20key%20value.

### Summary ###
Single digit latency at scale
Supports XML, JSON and HTML document formats
Supports Key/Value data models
Great for mobile, gaming, adtech and much much more
Serverless
Configureable with autoscaling
Data is stored on SSDs
Data is spread across 3 Geographic distinct DCs
Supports eventually consistent reads and strongly consistent reads
- Eventually - consistent within 1 second across all 3 GEOs (best for read performance)
- strongly - All 3 GEOs get data at once (Best for consisteny)

ACID transactions - Dynamo DB transaction types (https://en.wikipedia.org/wiki/ACID)
- A - atomic (all or nothing)
- C - consistent (consistent with data validation rules)
- I - isolated (must happen in isolation and not impact one another)
- D - durable (durable against error)

Primary Keys (can be primary key, partition key or HASH key)
- stores and retrieves data based on the key
- TWO types
- - ```partition key```
- - - OR 
- - ```composite primary key (partition key and sort key)```

### Partition key ###
- based on a unique attribute in table (customerID, e-mail, reg numer)
- - No two items can have the same partition key

### Composite Key ###
- items in a table may have the same partition key, but they have a different sort key.

Secondary indexes
- Query based on an attribute which is not the primary key
- Done with ```global secondary indexes``` and ```local secondary indexes```
- Allows you to run FAST queries on ```specific columns``` in a table (That is important)
- - You choose the columns you want included in the index, then run queries on the index, not the whole data set
- Keeps queries faster as it is run on a smaller dataset

### LOCAL SECONDARY INDEXES ###
- Can only be ```created``` and ```deleted``` when you create a Dynamo table
- Maximum of 5 per table
- Can't be created, modified or deleted AFTER table creation
- Same PARTITION key, different SORT key

### GLOBAL SECONDARY INDEX ###
- ```Create``` and ```delete``` whenever you like
- maximum of 20 per table (```Can be increased with a service limit ticket```)
- Separate PARTITION and SORT key for your data
- Since this is a new partition key you need to define additional RCU (Read Capacity Units) and WCU (Write capacity units) just for that Global secondary index
- - ```To avoid potential throttling```, the provisioned write capacity for a global secondary index should be equal or greater than the write capacity of the base table because new updates write to both the base table and global secondary index.

When thinking of entries in a Dynamo DB table
- ITEMS - Think of rows in the DB
- ATTRIBUTES - Think of columns in the DB

When adding ITEMS you specify if they are STRINGS ```"s"```or NUMBERS ```"n"```

once an item is created you CANNOT modify the partition key value (All other data values can be changed though)

SCAN 
- show everything in the table
- The Scan operation returns ```one or more items``` and item attributes by accessing ```every item in a table or a secondary index```. To have DynamoDB return fewer items, you can provide a ```FilterExpression``` operation

QUERY
- search for specific entries in a table

Access is managed using IAM
- you can use ```IAM conditions``` to restrict people to only access their own ITEMS in the dynamo DB table (See below)
- Remember the ```LeadingKeys``` parameter below, it is important!
"Condition": {
    "ForAllValues : StringEquals": {
        "dynamodb:LeadingKeys": [
            "${www.mygame.com:user_id}"
        ]
    }
}

### Query ###
- Query based on partition key AND OR a sort key too
- Returns all data assocaited with the item
- By default queries are ```eventually``` consistent. You can set a query to be stronlgy consistent though with parameters.
- You can use ```project expression``` to receive less data (i.e. specific attributes, not all attributes associated with your entry)
- - ```Projection Expression``` = ONLY GET SPECIFIC ATTRIBUTES (```columns```) FOR MATCHED QUERIES

- returns entries in ascending order
- - reverse the returned order by setting the ```ScanIndexForward``` configuration to false (```This only applies to QUERIES```, not scans)
- always sorted by the SORT key

### Scan ###
- Scans everything inside the table and gathers all attributes
- - Officially described with ```Return one or more items and item attributes by accessing every item in a table or a secondary index```
- Can use ```project expression``` to only return attributes you need
- You can use a filter with a scan too
- Functionality exists to perform ```parallel scans``` where two scans can run at once on a table
- - For large tables, a parallel scan can complete much faster than a sequential one, ```if the table size is 20 GB or larger``, the table's provisioned read throughput is not being fully used, and sequential Scan operations are too slow. Reference: Best Practices for Querying and Scanning Data.

```SCAN``` is less efficient that ```query```. Scan queries the whole table and dumps what is not needed. Query searches for the specific entries.
Scan can consume the provisioned throughput.

Increase performance by making a ```smaller page size``` which uses fewer read operations

Performance is best for ```query```, ```get``` or ```BatchGetItem``` queries.

BatchGetItem
- The BatchGetItem operation returns the attributes of one or more items from one or more tables. Reference: Amazon DynamoDB API Reference


```Provisioned throughput / capacity``` (used for predictable workload performance)
- 1 WCU (```Write capacity unit = 1 x 1KB per second```)
- -  1 RCU (Read capacity unit) = 1 x strongly consistent read of 4KB per second 
```OR``` 
- - 2 x eventually consistent reads of 4KB per second (This is default)
- provisioned throughput limits Can be increased with a service limit ticket

DynamoDB ```on-demand capacity```
- Dynamo DB instantly scales up and down based on application activity (No setting RCU and WCU)
- - good for unpredictable (spiky) workloads, apps where you do not know the perf profile or a pay per request model

### DAX (Dynamo DB accelerator) ###
- In memory cache for Dynamo DB 
- Only helps with ```READ``` performance (up to 10x)
- great for read heavy or bursty workloads
- Write Operations write to the CACHE and the backend table at the same time
- Configure API calls to query the ```cache cluster``` BEFORE hitting the dynamo table
- If the item is in the cache, it is classed as a ```cache hit``` and DAX returns the value to the requester
- If not in the cache then the DAX makes an ```eventually``` consistent get request to the dynamo table
- NOT suitable for 
- - ```strongly consistent``` reads
- - ```Write heavy``` workloads
- - Workloads that ```don't need micro seconds``` response times
- - Workloads that ```don't perform many read operations```

### Dynamo DB TTL ###
- Set expiry time on DB entries
- reduces costs by reducing table size
- You specify the attribute name for DynamoDB to check for the EPOCH time, for example could  be called expirationTIME(TTL)
- - the value is then expressed in ```EPOCH``` time format (Also known as ```unix``` time and ```posix``` time)
- - When you first configure it you can specify a ```run-preview``` which shows what items would be deleted based on the selected attribute
- when the time is greater than the expirationTimeTTL epoch time, entries are marked as expired and deleted within 48 hours
- - You can include a filter in scans and queries to not return items marked as expired

### Dynamo DB streams ###
- Time ordered sequence or stream of changes made to a Dynamo DB table (i.e. change modification info for all insert, update, delete)
- kept in a log and retained for ```24 hours```
- has a dedicated endpoint (separate to the actual dynamo table)
- primary key is recorded
- use cases
- - auditing
- - trigger events
- - replicate data across tables
- Near realtime
- Great event source for lambda

### Provisioned throughout and exponential backoff ###
- ```ProvisionedthroughputExceededException``` (not enough WCU and RCU), too many requests
- AWS SDK
- - auto retry requests until successful
- - uses exponential ntila backoffs out of the box



There is a dynamo DB encryption client
By default Dynamo DB encrypts at rest
Dynamo
- if your table is throttling scan requests you can ```reduce the page size``` (By default it is 1MB), 
- - This is done with a ```limit``` parameter which reduces the amount of data for each query and uses pauses between each request
- by default queries are eventually consistent

### Dynamo ###
- To read data from a table, you use operations such as GetItem, Query, or Scan. Amazon DynamoDB returns all the item attributes by default. To get only some, rather than all of the attributes, use a ```projection expression```.
- - A projection expression is a string that identifies the attributes that you want. To retrieve a single attribute, specify its name. For multiple attributes, the names must be comma-separated.
- - With ```project expression```, if you know the partition and sort key use QUERY
- - - SIMPLE TO REMEMBER - SCAN DOES NOT USE A PARTITION OR SORT KEY IN THE QUERY!!!!
- - With ```project expression```, if you ```DONT``` know the partition and sort key use SCAN
- ```BATCHGETITEM``` allows you to pass multiple partition keys to a query


Create a query with search criteria using:
- key condition expression
- partition key name in the equality condition

To get all the consumed capacity in a dynamo DB request you need to include:
- ReturnConsumedCapacity
- and set the parameter to ```total```