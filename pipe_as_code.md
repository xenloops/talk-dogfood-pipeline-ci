# The story so far

So far we've seen a cobbling together of a couple of security tools getting run as tasks by Jenkins. Seems to work pretty well, but... 
* The configuration wasn't straightforward
* Difficult to understand without screenshots
* What happens when disaster strikes, taking your Jenkins install out?

<br />
<br />
<br />
<br />
<br />
<br />

# A better way

What if we could build this pipeline as a single file?
* Easy to follow
* Easy to change
* Easy to copy between projects
* Easy to backup and restore
* Maintain in project repository as a code file

<br />
<br />
<br />
<br />
<br />
<br />

## The basic Jenkinsfile
```
pipeline {
    agent any

    stages {
        stage('Stage 1') {
            steps {
                // Stage 1 command(s) here
            }
        }
        stage('Stage 2') {
            steps {
                // Stage 2 command(s) here
            }
        }
        ...
        stage('Stage n') {
            steps {
                // Stage n command(s) here
            }
        }
    }
}
```

Some things to keep in mind:
* Commands need to be handled differently in Jenkinsfile
* Any service account keys or passwords should be imported

Example actual [Jenkinsfile](https://github.com/xenloops/password-vault/blob/master/Jenkinsfile)
