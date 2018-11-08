If your service has had issues installing or uninstalling, you can use the framework cleaner docker image, mesosphere/janitor or janitor.py script, to simplify the process of removing your service instance from ZooKeeper/mesos and destroying all the data associated with it.

**states that may require janitor**
  - inconsistancies between mesos, marathon, and/or zookeeper
     - service state is Deleting in DC/OS Services UI but listed as a completed framework in https://{dcos_url}/mesos
     - service does not exist in DC/OS but is listed in Mesos as Active Framework.
     - service has been deleted but appears in exhibitor https://{dcos_url}/exhibitor
     
**signs that janitor may be needed:**

* **at install**
   - if a framework will not deploy and in the debug interface or logs a message states that the framework or service name already exhists 

* **at uninstall**
   - if after uninstall the service remains in the state of **Deleting**
      - may or not appear healthy in https://{dcos_url}/marathon 

* after running "dcos package uninstall {package_name}" you recieve a message:
  - **"Incomplete uninstall of package [{package_name}] due to Mesos unavailability"**


**additional checks**
* The service may be inactive and will not be shown in the DC/OS UI, but you can find it by using this CLI command:
   - -- dcos service --inactive
* Reservations on a slave for a completed service not successfully removed
    - run status.py script and search for the framework in question
       -https://github.com/justinrlee/dcos-toys/blob/master/dcos-status/status.py
         

**Manual cleanup of zookeeper**
1. Open http://url-for-your-master-node/exhibitor
2. Explorer tab, expand '/' folder
3. Check for an entry named 'dcos-service-cassandra'. If 'dcos-service-cassandra' exists then the janitor failed to complete.
4. To manually delete 'dcos-service-cassandra' (note: this cannot be undone):
5. Select 'dcos-service-cassandra' from the list
6. Click 'modify' at the bottom of the page:
7. Type = Delete
8. Username / Ticket / Reason = any text you want (eg 'abc'), but they still need to be filled in
9. Click Next, then OK.
