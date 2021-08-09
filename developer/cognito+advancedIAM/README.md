### Web Identitiy federation ###
- Users can access AWS resources after authenticating with web based idp (facebook, google, amazon, facebook)
- following a successful auth users receive an AUTH CODE from idp
- users then trade this auth code for temp creds which enable access to AWS

```Amazon Cognito``` provides web ID federation and acts as an auth broker between your web app and the idp you use (i.e. FB). Means you don't need to write any additional code to allow FB users to sign up to your web app using their FB creds.
- supports multiple devices and tracks the devices used by users
- - uses ```push synchronization``` to push updates and syncs to devices
- - Uses SNS silent push notifications to push updates across all devices
- recommended approach for mobile devices
- The temporary credentials map to an IAM role
- means that no credentials need to be embedded on devices to allow users to access AWS resources

### Cognito terminology ###
- UserPools
- - User directories used to manage ```signup and signin``` functionality for mobile and web applications
- - HERE THE IDP WILL PROVIDE A JWT TOKEN

- sign-in
- - Users can sign in directly to the user pool or usin FB, AMAZON, GOOGLE

- Identity pools
- - Provide the ```temporary AWS credentials```. Enabling access to services like S3 and DYNAMO.
- - THE JWT RECEIVED FROM THE USERPOOL IS USED TO GET THE TEMPORARY CREDS
- - If you see ```configure access``` think about Identity pools

### github lab ###
- https://github.com/ACloudGuru-Resources/course-aws-certified-developer-associate/tree/main/Cognito_Demo

### IAM ###
- inline
- - policy which is embedded into the user, role or group it is assigned. Exists as a 1:2:1 link between the user and the policy. AN inline policy cannot be used by other entities, ONLY the user it has been assigned to.
- - Useful if you know only one role or person should only ever have this permissions

- customer managed poilcies
- - Custom policy you can create
- - ```AWS recommends managed policies``` over inline policies

- AWS managed policies
- - created and administered by AWS, created basedon job function or service
- - you cant change the permissions in a managed AWS policy

### STS assume role with web identity ###
- STS (Security token service)
- Provides temporary security tokens for users authenticated by a mobile or web using a web ID like google / amazon / facebook 
- Web apps can use the ```assume-role-with-web-identity``` api call using a JWT from the web IDP provider to make a call to STS. StS then provides temprary AWS creds which can be used.
- - A sample response will contain a ```assumed role user ARN``` &&  ```assumed role ID```. These can be queried and used programatically.
- - Then there is a session token section which includes ```token, accesskey, experiation and key ID```
- For mobile apps Cognito is recommended instead

### Advanced IAM ###
- to allow a user / group / role to assume a role in another account, the role should be assigned the ```sts:assumeRole``` permissions against the target accounts role. 
- The target Role to be assumed in the taregt account should also have permissions to allow the source account to assume it. This is set when it is created.

```AssumeRoleWithWebIdentity``` returns a set of temporary credentials (access key ID, secret access key and security token) which give temporary access to AWS services

### Remembered devices Cognito ###
Cognito enables developers to ```remember the devices on which end-users sign in to their application```. You can see the remembered devices and associated metadata through the console. In addition, you can build custom functionality using the notion of remembered devices. For example, with a content distribution application (e.g., video streaming), you can limit the number of devices from which an end-user can stream their content.

Cognito User Pools Provide
- Signup and SIgnin services
- A built in customizable UI to sign usres in
- Social sign in with FB, Google, Amazon as well as SAML sign in
- User directory mgmt and profiles
- MFA, phone and e-mail erification, account takeover
- workflows which trigger lambda

Cognito
- web identity federation 
- - no need for custom sign in code or to manage user identities. users login with their IDP, get an auth token, and exchange that for temp creds in AWS that map to an IAM role with permissions.
Cognito streams

- Amazon ```Cognito Streams``` gives developers control and insight into their ```data stored``` in Amazon Cognito. Developers can now configure a ```Kinesis stream``` to receive events as data is updated and synchronized. Amazon Cognito can push each dataset change to a Kinesis stream you own in real time.


Lambda & cognito:
- If you get a LambdaThrottledException exception when processing cognito events, ```you should retry the sync operation``` (update records).

Cognito
- can use a new or existing Kinesis stream to send cognito events
- - Cognito does need an IAM role though

- https://docs.aws.amazon.com/cognito/latest/developerguide/role-based-access-control.html
- - Rules are evaluated in order, and the IAM role for the first matching rule is used, unless ```CustomRoleArn``` is specified to override the order. For more information about user attributes in Amazon Cognito user pools, see Configuring User Pool Attributes.



### Use a user pool when you need to: ###
```AUTHENTICATION```
Design sign-up and sign-in webpages for your app.
Access and manage user data.
Track user device, location, and IP address, and adapt to sign-in requests of different risk levels.
Use a custom authentication flow for your app.

### Use an identity pool when you need to: ###
```AUTHORIZATION```
Give your users access to AWS resources, such as an Amazon Simple Storage Service (Amazon S3) bucket or an Amazon DynamoDB table.
Generate temporary AWS credentials for unauthenticated users.