## Cheat command ##

```aws codecommit create-repository --repository-name RepoFromCLI --repository-description "My demonstration repository"```

### code commit ###
- GIT (Source and Version control)
- If you see ```continuous integration``` in the EXAM think of ```code commit```
- Can integrate with SNS and cloudwatch for PR updates (comment, create, update etc)
- Repo permissions are done using IAM
- - Controls exist for Push, Pull, Branch, Merge, PR.
- You create git credentials for users on the IAM screen, under the ```Security Credentials``` tab. 
- - This creates a username and password that you can use with code commit
- - Similar to creating an access key (Can only download creds once and can only have 2 active creds at once)
- - - ```SSH access``` requires you to craete an SSH key and to upload it to the SSH Key screen on the same screen where you can download http creds for code commit.
- You can configure approval templates (minimum PR approvals before a merge can happen)
- - These can specify mandatory approvals from certain local IAM users or ARNs
- - These can be set on specific branches
- Can configure SNS triggers for REPO changes (all / commit / branch specific)
- - This can notify SNS topic or perform lambda changes

### Exam tips ###
- To create a notification when a pull request is made enable NOTIFICATIONS for PULL REQUEST UPDATE EVENTS. Then choose SNS for the notifications
- Triggers are used to send notifications when someone PUSHES to the repo only
- Approval templates (Rules for pull request approvals)
- All CodeCommit repos are ENCRYPTED using KMS (Which means developers need KMS permissions to commit and pull code)
