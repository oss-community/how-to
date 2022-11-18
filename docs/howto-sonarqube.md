# Sonarqube

---

## Sonarqube Server

<p align="justify">

Download [sonarqube](https://www.sonarqube.org/downloads/) and execute the following
command.

</p>

### Windows

```shell
mkdir C:\sdk\sonarqube
tar -xvf %HOMEPATH%\Downloads\sonarqube-*.zip --directory C:\sdk\sonarqube --strip-components=1
set SONARQUBE_HOME=C:\sdk\sonarqube
setx /M SONARQUBE_HOME C:\sdk\sonarqube
```

#### Start

To start the sonarqube, execute the following commands.

```shell
%SONARQUBE_HOME%/bin/windows-x86-64/StartSonar.bat
```

### Linux

```shell
sudo chown ${USER} -R /opt/
sudo chmod 765 -R /opt/
mkdir /opt/sonarqube
tar -xvf ~/Downloads/sonarqube-*.tar.gz --directory /opt/sonarqube --strip-components=1
sudo chmod +x -R /opt/sonarqube/bin/
echo "export SONARQUBE_HOME=/opt/sonarqube" >> ${HOME}/.bashrc
source ~/.bashrc
```

#### Start

To start the sonarqube, execute the following commands.

```shell
$SONARQUBE_HOME/bin/linux-x86-64/sonar.sh start
```

### Dashboard

In order to access dashboard of sonarqube, browse SonarQube URL therefore, for localhost installation it is
`http://localhost:9000`.

* username: admin
* password: admin

Generate token at _**My Account > security > Generate Tokens**_ then, you can use that in application configuration.

### Optional Part

Add sonarqube properties as environment variable (this part is optional).

#### Windows

```shell
setx /M SONAR_TOKEN token
setx /M SONAR_URL http://localhost:9000
```

#### Linux

```shell
echo "export SONAR_TOKEN=token" >> ${HOME}/.bashrc
echo "export SONAR_URL=http://localhost:9000" >> ${HOME}/.bashrc
```

---

## Sonar Scanner

<p align="justify">

Download [sonar scanner cli](https://binaries.sonarsource.com/?prefix=Distribution/sonar-scanner-cli/), then execute the
following commands.

</p>

### Windows

```shell
mkdir C:\sdk\sonar-scanner
tar -xvf %HOMEPATH%\Downloads\sonar-scanner-cli-*.zip --directory C:\sdk\sonar-scanner --strip-components=1
set SONAR_SCANNER_HOME=C:\sdk\sonar-scanner
setx /M SONAR_SCANNER_HOME=C:\sdk\sonar-scanner
setx /M PATH "%PATH%;%SONAR_SCANNER_HOME%\bin"
```

### Linux

```shell
sudo chown ${USER} -R /opt/
sudo chmod 765 -R /opt/
mkdir /opt/sonar-scanner
tar -xvf ~/Downloads/sonar-scanner-cli-*.tar --directory /opt/sonar-scanner --strip-components=1
sudo chmod +x -R /opt/sonar-scanner/bin/
echo "export SONAR_SCANNER_HOME=/opt/sonar-scanner" >> ${HOME}/.bashrc
# redhat base
sed -i 's/$PATH/$SONAR_SCANNER_HOME\/bin:$PATH/g' .bashrc
# debian base
sed -i 's/PATH=/PATH=$SONAR_SCANNER_HOME\/bin:/' .bashrc
source ~/.bashrc
```