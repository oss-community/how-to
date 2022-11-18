# Ngrok

---

## Installation

Go to [Ngrok](https://ngrok.com/) website then, create an account. After downloading ngrok, execute the following commands to install.

### Windows

```shell
mkdir C:\sdk\ngrok
tar -xvf %HOMEPATH%\Downloads\ngrok-v3-stable-windows-amd64.zip --directory C:\sdk\ngrok --strip-components=1
set NGROK_HOME=C:\sdk\ngrok
setx /M PATH "%PATH%;%NGROK_HOME%"
```

### Linux

```shell
sudo chown ${USER} -R /opt/
sudo chmod 765 -R /opt/
mkdir -p /opt/ngrok
tar -xvf ~/Downloads/ngrok-v3-stable-linux-amd64.tgz --directory /opt/ngrok --strip-components=1
sudo chmod +x -R /opt/ngrok/
echo "export NGROK_HOME=/opt/ngrok" >> ${HOME}/.bashrc
#redhat base
sed -i 's/$PATH/$NGROK_HOME:$PATH/g' .bashrc
#debian base
sed -i 's/PATH=/PATH=$NGROK_HOME:/' .bashrc
source ~/.bashrc
```

---

## Configuration

Next step is adding a token to ngrok configuration. The token is accessible in dashboard of your ngrok account.

```shell
ngrok config add-authtoken <token>
```

### Define Tunnel

Define tunnel like the following example in the config file. for windows, the path is `user-home\AppData\Local\ngrok\ngrok.yml`

```yaml
tunnels:
  app1:
    addr: port-number
    proto: http/tcp/...
  app2:
    addr: port-number
    proto: http/tcp/...
```

---

## Start

If you want to start all tunnels base on tunnels have defined in `ngrok.yml`

```shell
ngrok start --all 
```

If you want to start tunnels separately base on tunnels have defined in `ngrok.yml`

```shell
ngrok start app-name 
```

If you want to start only one tunnel without `ngrok.yml`, it is possible by following commands.

```shell
# ngrok http/tcp/... port-number
# example
ngrok http 8080
```

---

## Dashboard

In order to access to dashboard of Ngrok, browse Ngrok URL therefore, for localhost installation it is `http://127.0.0.1:4040`.