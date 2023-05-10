# Installing Jenkins
* Default Jenkins URL: http://localhost:8080
* Jenkins installation on Ubuntu Linux: Installation: https://www.jenkins.io/doc/book/installing/linux/#debianubuntu 
* Default Jenkins root dir: ```/var/lib/jenkins/```
* Default Jenkins working dir: ```/var/lib/jenkins/workspace/``` (bookmark this in file manager)

<br />

1. Download and install Java/OpenJDK 11 or 17 (I installed the v17 LTS release for better stability).
1. Download and import the Jenkins repository GPG keys (also see [this page](https://www.jenkins.io/blog/2023/03/27/repository-signing-keys-changing)):
    ```
    curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
      /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    ```
1. Add the Jenkins repository to the system:
    ```
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
      https://pkg.jenkins.io/debian binary/ | sudo tee \
      /etc/apt/sources.list.d/jenkins.list > /dev/null
    ```
1. Update the Linux package repositories: ```sudo apt-get update```
1. Install Jenkins: ```sudo apt-get install jenkins -y```
1. Finish setting up Jenkins. Browse to http://localhost:8080 and follow the instructions. To get the Jenkins initial unlock key, run ```sudo cat /var/lib/jenkins/secrets/initialAdminPassword```
1. Install the suggested plugins.
1. Install helper packages
    * git: ```sudo apt install git -y```

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

# Maintaining Jenkins

Some commands that will be essential for working with Jenkins in the long term.

* Start: ```sudo systemctl start Jenkins```
* Safe restart (new jobs queue up and run after the restart): ```http://localhost:8080/safeRestart```
* Restart (Without waiting for running jobs to complete): ```http://localhost:8080/restart```
* Stop (without waiting for jobs): ```sudo systemctl stop jenkins```
* To see or edit settings: ```systemctl cat jenkins```

<br /><br /><br /><br />

[Next slide](jenkins_demo.md)
