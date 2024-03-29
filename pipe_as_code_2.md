# Putting it all together

Let's build a basic CI pipeline, that:
1. Pulls a repository down
2. Builds the code
3. Scans for vulnerable dependencies
4. Scans for vulnerable code

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

## Build stage

* Essential for many SAST scanners for some languages like Java
* Good idea to make sure the code even compiles
* Command depends on method and operating system (e.g. Maven, Gradle, ADO)

        stage('Build') {
            steps {
                // Good to output something to the log:
                echo '*** Building the project...'
                sh 'ant compile jar'
            }
        }

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

## SCA stage

* Command depends on SCA software used
* DC takes a number of parameters:
   * ```-s``` path to scan (can be multiple)
   * ```--exclude``` path _not_ to scan
   * ```-o``` output file location (defaults to job directory)
   * ```-f``` report file types {CSV|JSON|XML|HTML|JUNIT|SARIF|PDF|ALL}
   * List of all options at https://jeremylong.github.io/DependencyCheck/dependency-check-cli/arguments.html
 * Specify name of the odcInstallation in Jenkins
 * Specify output file pattern

        stage ('SCA') {
            steps {
                echo '*** Checking dependencies...'
                dependencyCheck additionalArguments: ''' 
                    -s "./src"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'SCA: Dependency-Check'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

## SAST stage

* Command depends on SCA software used
* SQ expects a number of configuration settings that need to appear as parameters with ```-D```:
    * ```sonar.projectKey``` key for SonarQube to track the project (no spaces)
    * ```sonar.sources``` path to source files
    * ```sonar.language``` main language used
    * List of all options at https://docs.sonarqube.org/latest/analyzing-source-code/analysis-parameters (check for the version you're using)

```
stage('SAST') {
    environment {
        SCANNER_HOME = tool 'SonarQube Scanner'
        PROJECT_KEY = 'password-vault'
    }
    steps {
        echo '*** Scanning the code...'
        withSonarQubeEnv('SonarQube server') {
            sh '''$SCANNER_HOME/bin/sonar-scanner \
            -Dsonar.projectKey=$PROJECT_KEY \
            -Dsonar.projectName='Project analyzed by SonarQube' \
            -Dsonar.projectVersion=1.0 \
            -Dsonar.sources=src \
            -Dsonar.language=java \
            -Dsonar.sourceEncoding=UTF-8 \
            -Dsonar.java.binaries=build/classes/passvault \
            -Dsonar.java.libraries=dist/lib
            '''
        }
    }
 }
```

Example actual [Jenkinsfile](https://github.com/xenloops/password-vault/blob/master/Jenkinsfile)

<br /><br /><br /><br />

[Next slide](lessons_outcomes.md)
