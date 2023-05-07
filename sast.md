# Static Application Security Testing / Static Code Analysis

SAST analyzes the source code or binaries for common programming errors, coding standards violations, and known security weaknesses, and generates a report of potential vulnerabilities. SAST can be used early and throughout the SDLC to identify security issues before the application is deployed, and can help developers to improve code quality and reduce the likelihood of security breaches.

## SonarQube

SAST tool that scans for code smells, complexity, and vulnerabilties. It has both Community Edition (free/open-source) and commercial versions. 

Base site: https://www.sonarsource.com/products/sonarqube/
Documentation: https://docs.sonarqube.org/latest/
Downloads: https://www.sonarsource.com/products/sonarqube/downloads/

### Install

Instructions: https://docs.sonarqube.org/latest/setup/install-server

#### Database prerequisite

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

#### SonarQube

1. Download SonarQube Community Edition (I opted for the LTS version).
2. Create a new user and group:
    ```
    sudo groupadd sonarqube
    sudo useradd -s /bin/bash -m -c "sonarqube" -gsonarqube sonarqube
    ```
3. Unzip into desired directory and give recursive ownership to the new sonarqube user:
    ```
    sudo unzip sonarqube-9.9.0.65466.zip -d /opt
    sudo mv /opt/sonarqube-9.9.0.65466  /opt/sonarqube
    sudo chown -R sonarqube:sonarqube /opt/sonarqube-9.9.0.65466
    ```
4. Sonar makes some assumptions of people using the Community Edition. Like that you want to muck about in config files and change your user’s settings. (Well, it is free.) Change the following settings (per https://docs.sonarqube.org/latest/requirements/requirements -- make sure you’re looking at the appropriate version!)
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

1. Browse to the URL set in sonar.properties.

Check the files in ```/opt/sonarqube/logs``` if any troubles.

