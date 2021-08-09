Example snippets for cloudformation:
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/CHAP_TemplateQuickRef.html
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-sample-templates.html

Transform section:
Used to store reference code in S3 which you can reference in your CF stack.

AWSTemplatFormatVersion: 2010-09-09
parameters
conditions
resources -- Only mandatory section in a CF template
mapping
transforms
outputs

You can disable "Rollback on failure" so that you can debug if a CF deployment fails.

Cloudformation can hit limits the same as you can when using the console.

Use cloudtrail to log all CF call actions.

Stackpolicy  (JSON file which can be used to protect critical resources from unintentional updates caused by human error)
- So for example you can deploy a CF stack with EC2, S3, RDS and then add a stack polocy that PREVENTS a human from modifying the RDS instance with future cloudformation stack updates.

Failed rollack:
If you see "UPDATE_ROLLBACK_FAILED" you cannot update the stack
YOU SHOULD
fix the error before you can continue to rollback (May need to recreate resources manually mathcing what CF expects)

