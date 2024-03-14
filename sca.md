# Software Composition Analysis

* All realistically usable software uses functionality performed by other software.
* We depend on these building block libraries to build applications faster.
* When a dependency has a known, published vulnerability, our application inherits that vulnerability. 
* Therefore, we assume the risk of those dependencies.

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

# OWASP Dependency-Check

SCA tool that attempts to detect publicly disclosed vulnerabilities among a project's dependencies by determining if that dependency has any known vulnerabilities associated with it. If found, DC generates a report linking to the associated CVE entries.

Base project and downloads: https://github.com/jeremylong/DependencyCheck 

OWASP-MSP talk by DC's project lead Jeremy Long: https://www.youtube.com/watch?v=BUiWcDj1Ikw

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

## Install Dependency-Check

Jenkins integration: https://plugins.jenkins.io/dependency-check-jenkins-plugin

1. In Jenkins: Manage Jenkins > Plugins > Available Plugins
2. Search for “OWASP Dependency-Check” 
3. Check Install without Restart.
5. Manage Jenkins > Tools
6. Scroll down to Dependency-Check section
7. Click Add Dependency-Check button
8. Give it a name (e.g. “SCA: Dependency-Check”)
9. Click Install automatically
10. Click Add Installer
11. Pick the version you like (I recommend the latest)
12. Click Apply at the bottom

Restart Jenkins: ```http://localhost:8080/restart```

The first time you run it, it will take a while to download historical NVD data, which goes back to 2002!

2024 update: Beginning with version 9.x of Dependency-Check, you have the option of using an API key that you an request from NIST. After some trial and error, I recommend you don't; updating without a key may take longer, but using a key means you'll probably need to increase the timeout (which the slower non-key download already includes).

<br /><br /><br /><br />

[Next slide](dep-check_demo.md)
