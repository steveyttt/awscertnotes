cmk (customer master key) - https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html
- Can encrypt / decrypt up to 4KB of data
- used to generate / encrypt and decrypt ```data``` key
- Known as envelope encryption
- CMK keys cannot be exported out of AWS

DataKey
- used to encrypt / decrypt data

KMS is a regional service

KMS keys can:
- have alias names
- Have resource policies (Like IAM policies)
- - policies exist to create permissions for admin actions (like rolling keys) and for user actions (like decrypt/encrypt actions)
- - policies are for users or roles

If you delete a KMS key it takes one week to be deleted (incase you need it back)

CLI doco here:
https://awscli.amazonaws.com/v2/documentation/api/latest/reference/kms/index.html#cli-aws-kms

CLI info
### User Commands ###
```aws kms encrypt``` to encrypt a file
```aws kms decrypt``` to decrypt a file
```aws kms re-encrypt``` to re encrypt data with a new CMK

### Admin commands ###
```aws kms enable-key-rotation``` enable key rotation
```aws kms get-key-rotation-status``` get key rotation status
```aws kms generate-data-key``` Create a data key (Sometimes known as envelope key) (Use this to encrypt / decrypt data OVER 4 KB)

### Envelope encryption ###
- (Encrypt the key which encrypts the data)
- The CMK key encrypts the data key 
- The DATA key encrypts the data
- Used to encrypt anything over 4KB
- Using envelope encryption prevents everything going over the network to KMS
- Don't forget it is created with ```aws kms generate-data-key``` Create a data key (Sometimes known as envelope key) (Use this to encrypt / decrypt data OVER 4 KB)

Why do we use a CMK to encrypt / decrypt other keys in an AWS account which actually encypt the data?
Why not just use the CMK key to encrypt / decrypt all data?? The answer is bandwidth. If the CMK enc / dec all data it would need to process all data in the request (if data was TBs in size the requests would be huge.) By using KMS keys in an account the data does not need to traverse the AWS network and increases performance.

### Remember###
```CMKs do not encrypt things``` - They encrypt other keys
asymmetric encryption needs dedicated hardware HSM, it is a totally separate thing to KMS

```kms```
- With envelope encryption, unencrypted data is encrypted using a plaintext Data key. This key is further encrypted with a plaintext Master key.
- - The Master key is known as ```Customer Master Key``` and is stored in AWS KMS.
