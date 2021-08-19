Lifecycle hooks.

Enable you to run custom scripts when EC2 instances go through the ```EC2_INSTANCE_LAUCNHING``` phase and the ```EC2_INSTANCE_TERMINATING``` phase.
(Example is like domain nodes getting removed from AD)

When you configure lifecycle hooks, a call needs to be made to the autoscaling service when instances launch or terminate. Or a timeout value can be specified. (By default timeout is 1hour)

Nodes in an ASG can go into standby (do not take traffic from LB but remain in ASG for teams to do forensics)

Detach can also be done (Remove from ASG and turn into a stand alone instance)

- You can change the timeout for all hooks by using the HEARTBEAT parameter
- You can keep a server in the hook wait state for 48 hours
- - This is regardless of how many wait states you add!
- you complete the lifecycle hook with the call ```complete-lifecycle-action``` command
- You can use ```record-lifecycle-action-heartbeat``` command to add more time to the timeout

### COOL DOWN ###
- ASG will not provision additional nodes whilst new ones are going through the lifecycle hooks process
- If the hook fails on a node it enters the ABANDON state
- - this causes ASGs to terminate the instance and provison a new one

Can you lifecyclehooks with ```SPOT```
Spot still does lifecyclehooks when terminating

