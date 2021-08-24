dead letter queues
async functions are retried twice (if first run fails)
you can configure a dead letter queue (an sns or sqs target) to receieve the failed execution input
(Request ID, ErrorCode, ErrorMessage)

