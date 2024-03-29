# Static Application Security Testing / Static Code Analysis

SAST analyzes the source code or binaries for:

* Common programming errors
* Coding standards violations
* Known security weaknesses

SAST can be used early and throughout the SDLC to:

* Identify defects in code before the application is deployed
* Help developers learn and improve the quality of their code
* Reduce the likelihood of security breaches

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

## SonarQube

SAST tool that scans for code smells, complexity, and vulnerabilties. It has both Community Edition (free/open-source) and commercial versions. 

* Base site: https://www.sonarsource.com/products/sonarqube/
* Documentation: https://docs.sonarqube.org/latest/
* Downloads: https://www.sonarsource.com/products/sonarqube/downloads/
* Installation: https://docs.sonarqube.org/latest/setup/install-server (adjust for your version)

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

### Install the database prerequisite

1. Install PostgreSQL from the system (SQ also supports MSSQL and Oracle):
    ```sudo apt install postgresql -y```
2. Switch to postgres user:
    ```sudo su - postgres```
3. Create an empty schema:
    ```createdb default```
4. Create a sonarqube user (enter password when prompted):
    ```createuser --pwprompt sonarqube```
5. Return to regular user:
    ```su - [username]```
6. Get into the Postgres command prompt (you’ll see a ```postgres=#``` prompt):
    ```sudo -u postgres psql```
7. Create the Sonarqube DB (this should give the user the correct permissions):
    ```
    CREATE DATABASE sonarqube WITH ENCODING 'UTF8' OWNER sonarqube TEMPLATE=template0;
    CREATE SCHEMA sonarqubeSchema;
    GRANT ALL ON SCHEMA sonarqubeSchema TO sonarqube;
    alter user sonarqube with createdb;
    ```
8. See list of DB users and their roles:
    ```\du;```
9. Exit Postgres command prompt:
    ```quit```

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

### Install SonarQube

1. Download SonarQube Community Edition (I opted for the latest LTS version).
2. Create a new user and group:

        sudo groupadd sonarqube
        sudo useradd -s /bin/bash -m -c "sonarqube" -gsonarqube sonarqube

3. Unzip into desired directory and give recursive ownership to the new sonarqube user:

        sudo unzip sonarqube-9.9.0.65466.zip -d /opt
        sudo mv /opt/sonarqube-9.9.0.65466  /opt/sonarqube
        sudo chown -R sonarqube:sonarqube /opt/sonarqube-9.9.0.65466

4. Change the following settings (per https://docs.sonarqube.org/latest/requirements/requirements -- make sure you’re looking at the appropriate version!)
    Add to /etc/sysctl.conf:
    
        vm.max_map_count=524288
        fs.file-max=131072
        ulimit -n 131072
        ulimit -u 8192

    Add to /etc/security/limits.conf:

        sonarqube   -   nofile   131072
        sonarqube   -   nproc    8192

    Edit $SONARQUBE-HOME/conf/sonar.properties:

        sonar.jdbc.username=sonarqube
        sonar.jdbc.password=[password]
        sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
        sonar.web.host=127.0.0.1
        #sonar.web.context=/sonarqube
        sonar.web.port=9000

5. Set up the Sonarqube system service as shown in https://docs.sonarqube.org/latest/setup/operate-server/: 

    Create a file ```/etc/systemd/system/sonarqube.service``` and populate with:

        [Unit]
        Description=SonarQube service
        After=syslog.target network.target

        [Service]
        Type=simple
        User=sonarqube
        Group=sonarqube
        PermissionsStartOnly=true
        ExecStart=/bin/nohup [path]/java/bin/java -Xms32m -Xmx32m 
          -Djava.net.preferIPv4Stack=true -jar [path]/sonarqube/lib/sonar-application-[ver].jar
        StandardOutput=syslog
        LimitNOFILE=131072
        LimitNPROC=8192
        TimeoutStartSec=5
        Restart=always
        SuccessExitStatus=143

        [Install]
        WantedBy=multi-user.target

    (One inconvenience is that the version number needs updating whenever jar file version changes.)

    Enable, then start the new service:

        sudo systemctl enable sonarqube.service
        sudo systemctl start sonarqube.service

1. Browse to the URL set in sonar.properties (e.g. http://127.0.0.1:9000 or http://localhost:9000)

Check the files in ```/opt/sonarqube/logs``` if any troubles.

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

### Integrate SonarQube with Jenkins

https://docs.sonarqube.org/9.9/analyzing-source-code/ci-integration/jenkins-integration (change URL for version you're using)

1. In Jenkins: Manage Jenkins > Plugins > Available Plugins
2. Search for “SonarQube Scanner” 
3. Check the selection box
4. Install and restart: ```http://localhost:8080/restart```
5. After restarting Jenkins, follow the instructions at:
    * https://docs.sonarqube.org/9.9/analyzing-source-code/scanners/jenkins-extension-sonarqube
    * https://docs.sonarqube.org/9.9/analyzing-source-code/ci-integration/jenkins-integration
7. Manage Jenkins > System > Manage Credentials > System store > Global credentials (unrestricted)
8. Add credentials:
    1. Kind: Secret Text
    1. Scope: Global
    2. To get the Secret, copy and paste the token here from SonarQube: User > My Account > Security > Generate a token (Name: Jenkins; Type: Global Analysis Token; No expiration).
9. Back in Jenkins: Manage Jenkins > System > SonarQube servers: 
    1. Add SonarQube
    1. Name: SonarQube server
    2. Server URL: http://localhost:9000
    3. Server authentication token: Secret text
    4. Click Save.
10. Dashboard > Manage Jenkins > Tools > SonarQube Scanner installations: 
    1. Add SonarQube Scanner
    2. Name: SonarQube Scanner 
    3. **Uncheck** Install automatically
    4. Set ```SONAR_RUNNER_HOME: /opt/sonarqube/bin/linux-x86-64```
    5. Click Save.

<br /><br /><br /><br />

[Next slide](sonarqube_demo.md)
