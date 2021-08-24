Keep in mind IAM conditions which can use tags

"Condition" : {
    "StringEquals" : {"ec2:ResourceTag/Owner" : "${aws:username}"}
}

Cost allocation tags can be enabled from the billing console.
Get a csv file for spend
Cost and Usage reports need to go into an S3 bucket
