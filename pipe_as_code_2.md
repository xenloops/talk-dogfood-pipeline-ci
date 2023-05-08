# Putting it all together



## Build stage

        stage('Build') {
            steps {
                echo '*** Building the project...'
                sh 'ant compile jar'
            }
        }


## SCA stage

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


## 


Example actual [Jenkinsfile](https://github.com/xenloops/password-vault/blob/master/Jenkinsfile)

[Next slide](pipe_as_code_2.md)
