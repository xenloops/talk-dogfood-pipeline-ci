# Jenkins demos

1. Click Create a Job
2. Name the job (e.g. the repo's name)
3. Freestyle Project
4. Click OK
5. General section: GitHub Project URL
6. Source Code Management section: Git > Repository URL
7. Branch Specifier: [blank]


## List files and backup directory


## Build HelloWorld code

In Jenkins:
1. New Item
2. Name: Hello World
3. Freestyle project
4. Click OK
In Configure screen:
1. General: Check GitHub project
2. Project url (e.g. https://github.com/xenloops/jenkins-nude)
3. Source Code Management: check Git, enter Project url again
4. Branches to build: clear to blank to build any branches
5. Build Steps: Command: ```javac Main.java```
6. Click Save
7. In Project page, click Build Now.
8. After about 1/4 second, the #1 build will complete. Check the Jenkins working directory for the new project and confirm that the repo is there, as well as a new ```Main.class``` file.

[Next slide](sca.md)
