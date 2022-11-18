# JFrog

---
<p align="justify">

Download [JFrog](https://jfrog.com/community/open-source//) and execute the following command.

</p>

## Windows

```shell
mkdir C:\sdk\jfrog
tar -xvf %HOMEPATH%\Downloads\jfrog-artifactory-oss-*.zip --directory C:\sdk\jfrog --strip-components=1
set JFROG_HOME=C:\sdk\jfrog
setx /M JFROG_HOME C:\sdk\jfrog
```

### Start

```shell
%JFROG_HOME%\app\bin\artifactory.bat
```

## Linux

```shell
sudo chown ${USER} -R /opt/
sudo chmod 765 -R /opt/
mkdir /opt/jfrog
tar -xvf ~/Downloads/jfrog-artifactory-oss-*.tar --directory /opt/jfrog --strip-components=1
sudo chmod +x -R /opt/jfrog/app/bin/
echo "export JFROG_HOME=/opt/jfrog" >> ${HOME}/.bashrc
source ~/.bashrc
```

### Start

```shell
$JFROG_HOME/artifactory/app/bin/artifactoryctl
```

## Dashboard

In order to access to dashboard of JFrog, browse JFrog URL therefore, for localhost installation it is
`http://localhost:8082/ui`.

* username: admin
* password: password

## Initialization

Follow the instruction for initialization.

* Change password at _**Administration > User Management > Users**_.
* Get encrypted password from _**Edit Profile**_ menu.
* Create repository by _**Quick Setup**_ menu.