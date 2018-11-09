# Changing SSH Keys For Service Accounts
**This is not tested for all frameworks and should be thoughroughly tested prior to attempting in a production environment for each service**

DC/OS does not have an update command to change the public key associated. In order to acomplish this 

1. Delete Service Account
2. Recreate Service Account with new public key
3. Update the Secret used by the service to contain private key
4. Restart the scheaduler for the framework


**Frameworks and why this can work:**

* The Framework scheaduler is responsible for achieving the target state of a framework. When the configuration of a framework matches the desired configuration, the Scheduler will tell Mesos to suspend sending new offers, as there’s nothing to be done. The Scheduler then idles until it; 
   - receives an RPC from Mesos notifying it of a task status change, 
   - receives an RPC from an end user against one of its HTTP APIs, 
   - is killed by Marathon as the result of a configuration change.

**How DC/OS Handles Framework Reconfiguration Nativly**
This is the flow for reconfiguring a DC/OS service either in order to update specific configuration values, or to upgrade it to a new package version.
Steps handled by the DC/OS cluster
1. The user edits the Scheduler’s environment variables either using the Scheduler CLI’s update command or via the DC/OS GUI.
2. The DC/OS package manager instructs Marathon to kill the current Scheduler and launch a new Scheduler with the updated environment variables.

**Note of Caution**
The ssl key rotate steps outlined above conform to a similar model as the native framework update and should be able to be executed in under a minute. it is importaint to note that once the older service account has been deleted the schedular will not be able to make any changes or updates to the framework until the process is complete.  





