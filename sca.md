# Software Composition Analysis

* All realistically usable software uses functionality performed by other software.
* We depend on these building block libraries to build applications faster.
* When a dependency has a known, published vulnerability, our application inherits that vulnerability. 
* 

<br />
<br />
<br />
<br />

# OWASP Dependency-Check

SCA tool that attempts to detect publicly disclosed vulnerabilities among a project's dependencies by determining if that dependency has any Common Vulnerability Enumerations (CVE) associated with it. If found, DC generates a report linking to the associated CVE entries.

Base project and downloads: https://github.com/jeremylong/DependencyCheck 

OWASP-MSP talk by DC's project lead Jeremy Long: https://www.youtube.com/watch?v=BUiWcDj1Ikw

<br />
<br />
<br />
<br />

## Install 


Integrate with Jenkins 
https://plugins.jenkins.io/dependency-check-jenkins-plugin/

In Jenkins: Manage Jenkins > Plugins > Available Plugins, search for “OWASP Dependency-Check” Check, then Install without Restart
Manage Jenkins > Tools, scroll down to Dependency-Check section. Click Add Dependency-Check button. Give it a name (e.g. “SCA: Dependency-Check”), then click Install automatically. Click Add Installer, then pick the version you like (I recommend the latest). Click Apply at the bottom.
Restart Jenkins: localhost:8080/restart

Arguments: (not really needed; see SAMM project)
--format: ALL
--scan “[path]” (e.g. “/var/lib/jenkins/workspace/vulnerable-sso”)
--prettyPrint

To test it out:
Some projects that give results without trying too hard:
https://github.com/OwlCarousel2/OwlCarousel2
https://github.com/williamfiset/DEPRECATED-data-structures

Click Create a Job
Name: Test
Freestyle Project
On General settings page:
Build Steps: add Invoke Dependency-Check with these arguments:
...
Post-build Actions: add Publish Dependency-Check results
Click Save.
Back on project page, click Build Now.

Watch progress: click build >  Console Output
Results: Project page > Latest Dependency-Check 

The first time you run it, it will take a while to download historical NVD data – goes back to 2002!
