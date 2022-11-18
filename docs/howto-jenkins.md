# Jenkins

---
Download [jenkins](https://www.jenkins.io/download/) as a war file and execute the following command.

## Windows

```shell
mkdir C:\sdk\jenkins
move %HOMEPATH%\Downloads\jenkins.war C:\sdk\jenkins
setx /M JENKINS_HOME C:\sdk\jenkins

```

### Start

```shell
java -jar %JENKINS_HOME%\jenkins.war --httpPort=8080
```

## Linux

```shell
sudo chown ${USER} -R /opt/
sudo chmod 765 -R /opt/
mkdir /opt/jenkins
mv ~/Downloads/jenkins.war /opt/jenkins
echo "export JENKINS_HOME=/opt/jenkins" >> ${HOME}/.bashrc
```

### Start

```shell
java -jar $JENKINS_HOME/jenkins.war --httpPort=8080
```

## Dashboard

In order to access to dashboard of Jenkins, browse Jenkins URL therefore, for localhost installation it is
`http://localhost:8080`.

* username: admin
* password: look at the console