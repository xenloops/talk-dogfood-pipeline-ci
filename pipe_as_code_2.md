# Putting it all together

Let's build a basic CI pipeline, that:
1. Copies a repository
2. Builds the code
3. Scans for vulnerable dependencies
4. Scans for vulnerable code

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

## Build stage

* Essential for many SAST scanners for Java
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
        * -o
        * -s: source file location
        * -f: report file types {PDF|...|...|ALL}
 * Specify name of the odcInstallation in Jenkins
 * Specify output file pattern

        stage ('SCA') {
            steps {
                echo '*** Checking dependencies...'
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'SCA: Dependency-Check'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

## SAST stage

        stage('SAST') {
            environment {
                SCANNER_HOME = tool 'SonarQube Scanner'
                PROJECT_KEY = 'password-vault'
            }
            steps {
                echo '*** Scanning the code...'
                // test command here
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

<br /><br /><br /><br />

Example actual [Jenkinsfile](https://github.com/xenloops/password-vault/blob/master/Jenkinsfile)

[Next slide](pipe_as_code_2.md)
