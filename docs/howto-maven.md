
# Maven

---
## Windows

Download [Maven](https://maven.apache.org/download.cgi) in zip format.

```shell
mkdir C:\sdk\maven
tar -xvf %HOMEPATH%\Downloads\apache-maven-*-bin.zip --directory C:\sdk\maven --strip-components=1
set M2_HOME=C:\sdk\maven
setx /M M2_HOME C:\sdk\maven
setx /M PATH "%PATH%;%M2_HOME%\bin"
```

## Linux

Download [Maven](https://maven.apache.org/download.cgi) in tar.gz format.

```shell
sudo chown ${USER} -R /opt/
sudo chmod 765 -R /opt/
mkdir /opt/maven
tar -xvf ~/Downloads/maven*.tar.gz --directory /opt/maven --strip-components=1
sudo chmod +x -R /opt/maven/bin/
echo "export M2_HOME=/opt/maven" >> ${HOME}/.bashrc
# redhat base
sed -i 's/$PATH/M2_HOME\/bin:$PATH/g' .bashrc
# debian base
sed -i 's/PATH=/PATH=$M2_HOME\/bin:/' .bashrc
source ~/.bashrc
```

## Test Maven

```shell
mvn -version
```
