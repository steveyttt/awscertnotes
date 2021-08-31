Play with the jobstatuspoller

- visualize serverless applications
- output from one part of step function and be input to the next

step function state machine encompasses all lambda functions
- each function in a state machine is called a ```step function task```

- When a state reports an error, the default course of action for AWS Step Functions is to log the error and perform a single retry after 1 second. If that doesn't succeed, AWS Step Functions will fail the execution entirely.