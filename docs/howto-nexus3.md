# Nexus3

---
<p align="justify">

Download [Nexus3](https://help.sonatype.com/repomanager3/product-information/download). In the extracted path execute
the following command.

</p>

## Windows

```shell
mkdir C:\sdk\sonatype
tar -xvf %HOMEPATH%\Downloads\nexus-3.*-win64.zip --directory C:\sdk\sonatype
ren C:\sdk\sonatype\nexus-* nexus
set NEXUS_HOME=C:\sdk\sonatype
setx /M NEXUS_HOME C:\sdk\sonatype
```

### Start

```shell
%NEXUS3_HOME%\nexus\bin\nexus /run
```

## Linux

```shell
sudo chown ${USER} -R /opt/
sudo chmod 765 -R /opt/
mkdir -p /opt/sonatype
tar -xvf ~/Downloads/nexus-*-unix.tar.gz.tar --directory /opt/sonatype
mv /opt/sonatype/nexus-* /opt/sonatype/nexus
sudo chmod +x -R /opt/sonatype/nexus/bin/
echo "export NEXUS_HOME=/opt/sonatype" >> ${HOME}/.bashrc
source ~/.bashrc
```

### Start

```shell
$NEXUS_HOME/nexus/bin/nexus run
```

## Dashboard

In order to access to dashboard of Nexus, browse Nexus URL therefore, for localhost installation it is
`http://localhost:8081`. Also, for changing the configuration you can modify `nexus-default.properties` under `etc`
directory.

* username: admin
* password: print on the console