# Lessons learned

* Installing and updating Jenkins is easy
* Installing and integrating other tools less so
* If running tools rarely, expect delays for updates
  * Automating project to run regularly alleviates this a bit
* Error messages in Jenkins Console Output might be useless
  * Look in logs in ```/var/lib/jenkins/logs``` to troubleshoot
* Writing intentially vulnerable code can be tricky
  * GitHub is full of deprecated projects; look for:
    * ```DEPRECATED``` in the name
    * Not recently maintained
    * Filter for languages most used by organization
    * Projects that use security features, e.g.
      * Encryption
      * Authentication
      * Sensitive data storage (e.g. need a database)

# 
