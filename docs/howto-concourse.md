# Concourse

---

## Environment variable

As a first step, create a workspace directory and define environment variables in .env file.<br/>

### Windows

```shell
mkdir %HOMEPATH%\concourse-workspace
cd %HOMEPATH%\concourse-workspace

echo #env > .env
```

### Linux

```shell
mkdir ~/concourse-workspace
cd ~/concourse-workspace

echo #env > .env
```

```dotenv
CONCOURSE_DB=concourse
CONCOURSE_DB_HOST=concourse-db
CONCOURSE_DB_PORT=5432
CONCOURSE_DB_USER=concourse_user
CONCOURSE_DB_PASSWORD=concourse_pass
CONCOURSE_EXTERNAL_URL=http://localhost:8080
CONCOURSE_CLUSTER_NAME=Company-name
CONCOURSE_ADD_LOCAL_USER=user
CONCOURSE_ADD_LOCAL_PASSWORD=pass
CONCOURSE_MAIN_TEAM_LOCAL_USER=user
CONCOURSE_TSA_HOST=concourse-web:2222
```

---

## Certificate

Then, the certificates must be generated and move them to a directory in order to use that as a directory mapping.<br/>

### Windows

```shell
mkdir %HOMEPATH%\concourse-workspace\keys

call ssh-keygen -t rsa -b 4096 -m PEM -f %HOMEPATH%\concourse-workspace\keys\session_signing_key
call ssh-keygen -t rsa -b 4096 -m PEM -f %HOMEPATH%\concourse-workspace\keys\tsa_host_key
call ssh-keygen -t rsa -b 4096 -m PEM -f %HOMEPATH%\concourse-workspace\keys\worker_key

ren %HOMEPATH%\concourse-workspace\keys\worker_key.pub authorized_worker_keys
```

### Linux

```shell
mkdir ~/concourse-workspace/keys

ssh-keygen -t rsa -b 4096 -m PEM -f ~/concourse-workspace/keys/session_signing_key
ssh-keygen -t rsa -b 4096 -m PEM -f ~/concourse-workspace/keys/tsa_host_key
ssh-keygen -t rsa -b 4096 -m PEM -f ~/concourse-workspace/keys/worker_key

mv ~/concourse-workspace/keys/worker_key.pub ~/concourse-workspace/keys/authorized_worker_keys
```

---

## Docker Configuration

After generating keys, create the following `docker-compose.yml` file in the workspace to execute by Docker/Podman
compose.

```yaml
  concourse-db:
    container_name: concourse-db
    hostname: concourse-db
    restart: always
    image: postgres
    environment:
      POSTGRES_DB: ${CONCOURSE_DB}
      POSTGRES_PASSWORD: ${CONCOURSE_DB_PASSWORD}
      POSTGRES_USER: ${CONCOURSE_DB_USER}
      PGDATA: /database
    ports:
      - "5432:5432"
    volumes:
      - ~/concourse-workspace/concourse_postgresql:/var/lib/postgresql
    ulimits:
      nproc: 262144
      nofile:
        soft: 32000
        hard: 40000
  concourse-web:
    image: concourse/concourse
    container_name: concourse-web
    hostname: concourse-web
    restart: always
    command: web
    privileged: true
    depends_on: [ concourse-db ]
    ports: [ "8080:8080" ]
    volumes:
      - ~/concourse-workspace/keys:/keys
    environment:
      CONCOURSE_POSTGRES_HOST: ${CONCOURSE_DB_HOST}
      CONCOURSE_POSTGRES_PORT: ${CONCOURSE_DB_PORT}
      CONCOURSE_POSTGRES_DATABASE: ${CONCOURSE_DB}
      CONCOURSE_POSTGRES_USER: ${CONCOURSE_DB_USER}
      CONCOURSE_POSTGRES_PASSWORD: ${CONCOURSE_DB_PASSWORD}
      CONCOURSE_ADD_LOCAL_USER: ${CONCOURSE_ADD_LOCAL_USER}:${CONCOURSE_ADD_LOCAL_PASSWORD}
      CONCOURSE_MAIN_TEAM_LOCAL_USER: ${CONCOURSE_MAIN_TEAM_LOCAL_USER}
      CONCOURSE_EXTERNAL_URL: ${CONCOURSE_EXTERNAL_URL}
      CONCOURSE_CLUSTER_NAME: ${CONCOURSE_CLUSTER_NAME}
      CONCOURSE_SESSION_SIGNING_KEY: /keys/session_signing_key
      CONCOURSE_TSA_HOST_KEY: /keys/tsa_host_key
      CONCOURSE_TSA_AUTHORIZED_KEYS: ./keys/authorized_worker_keys
  concourse-worker:
    image: concourse/concourse
    container_name: concourse-worker
    hostname: concourse-worker
    restart: always
    command: worker
    privileged: true
    depends_on: [ concourse-web ]
    volumes:
      - ~/concourse-workspace/keys:/keys
    environment:
      CONCOURSE_WORK_DIR: /var/lib/concourse
      CONCOURSE_TSA_HOST: ${CONCOURSE_TSA_HOST}
      CONCOURSE_TSA_PUBLIC_KEY: /keys/tsa_host_key.pub
      CONCOURSE_TSA_WORKER_PRIVATE_KEY: /keys/worker_key
      CONCOURSE_CONTAINERD_DNS_PROXY_ENABLE: "true"
      CONCOURSE_CONTAINERD_DNS_SERVER: "1.1.1.1,8.8.8.8"
      CONCOURSE_RUNTIME: "containerd"
      CONCOURSE_BAGGAGECLAIM_DRIVER: "naive"
```

### Start

Finally, deploy the services to Docker/Podman.

```shell
docker compose --file docker-compose.yml --project-name concourse --env-file .env up --build -d
```

---

## Dashboard

In order to access to dashboard of Concourse, browse Concourse URL therefore, for localhost installation it is
`http://localhost:8080`.

* Username: user
* Password: pass

---

## FLY

Download FLY CLI from concourse dashboard and execute the following commands.

### Windows

```shell
# solution 1: download via web browser
mkdir C:\sdk\concourse
copy %HOMEPATH%\Downloads\fly C:\sdk\concourse
setx /M FLY_HOME=C:\sdk\concourse
setx /M PATH "%PATH%;%FLY_HOME%"
```

```shell
# solution 2
% $concoursePath = 'C:\sdk\concourse\'; mkdir $concoursePath; `
[Environment]::SetEnvironmentVariable('PATH', "$ENV:PATH;${concoursePath}", 'USER'); `
$concourseURL = 'http://localhost:8080/api/v1/cli?arch=amd64&platform=windows'; `
Invoke-WebRequest $concourseURL -OutFile "${concoursePath}\fly.exe"
```

### Linux

```shell
# solution 1: download via web browser
sudo chown ${USER} -R /opt/
sudo chmod 765 -R /opt/
mkdir -p /opt/concourse
cp ~/Downloads/fly /opt/concourse
sudo chmod +x -R /opt/concourse/
echo "export FLY_HOME=/opt/concourse" >> ${HOME}/.bashrc
#redhat base
sed -i 's/$PATH/$FLY_HOME:$PATH/g' .bashrc
#debian base
sed -i 's/PATH=/PATH=$FLY_HOME:/' .bashrc
source ~/.bashrc
```

```shell
# solution 2
curl 'http://localhost:8080/api/v1/cli?arch=amd64&platform=linux' -o fly && chmod +x ./fly && mv ./fly /usr/local/bin/
```

### Login

To login via Concourse CLI (fly), execute the following commands.

```shell
fly --target pipeline-name login --team-name main --concourse-url http://localhost:8080 -u user -p pass
```

---

## Hello World

If you want to install Concourse as a base, download the following file to create an instance of Concourse.

```shell
curl -O https://concourse-ci.org/docker-compose.yml
```

Then, deploy the Concourse service in Docker/Podman and login via browser also, FLY.

```shell
docker-compose up -d
```

```shell
fly --target tutorial login --concourse-url http://localhost:8080 -u test -p test
```

After that, create a job for Concourse in a yml file named `pipeline-file.yml`.

```yaml
jobs:
  - name: hello-world-job
    plan:
      - task: hello-world-task
        config:
          # Tells Concourse which type of worker this task should run on
          platform: linux
          # This is one way of telling Concourse which container image to use for a
          # task. We'll explain this more when talking about resources
          image_resource:
            type: registry-image
            source:
              repository: busybox # images are pulled from docker hub by default
          # The command Concourse will run inside the container
          # echo "Hello world!"
          run:
            path: echo
            args: [ "Hello world!" ]
```

To execute the job follow the commands.

```shell
fly -t tutorial set-pipeline -p hello-world -c pipeline-file.yml
fly -t tutorial unpause-pipeline -p hello-world
fly -t tutorial trigger-job --job hello-world/job-name --watch
```