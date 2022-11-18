# Java

---
This tutorial has written for jdk 17 but, it is possible to change the other version.

## Windows

Download [Java 17](https://www.oracle.com/java/technologies/downloads/#jdk17-windows) in zip format.

```shell
mkdir C:\sdk\jdk-17
tar -xvf %HOMEPATH%\Downloads\jdk-17_windows-x64_bin.zip --directory C:\sdk\jdk-17 --strip-components=1
set JAVA_HOME=C:\sdk\jdk-17
setx /M JAVA_HOME C:\sdk\jdk-17
setx /M PATH "%PATH%;%JAVA_HOME%\bin"
```

## Linux

Download [Java 17](https://www.oracle.com/java/technologies/downloads/#jdk17-linux) in tar.gz format.

```shell
sudo chown ${USER} -R /opt/
sudo chmod 765 -R /opt/
mkdir -p /opt/jdk-17
tar -xvf ~/Downloads/jdk-17_linux-x64_bin.tar.gz --directory /opt/jdk-17 --strip-components=1
sudo chmod +x -R /opt/java-17/bin/
echo "export JAVA_HOME=/opt/jdk-17" >> ${HOME}/.bashrc
#redhat base
sed -i 's/$PATH/$JAVA_HOME\/bin:$PATH/g' .bashrc
#debian base
sed -i 's/PATH=/PATH=$JAVA_HOME\/bin:/' .bashrc
source ~/.bashrc
```

## Test Java

```shell
java -version
```
