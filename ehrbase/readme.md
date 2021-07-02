# Docker EHRBase App + EHRBase DB

## Prerequisites
- Docker must be installed and running on your computer
> The Docker proxy setting must be configured if your network has internet access through a proxy

## Docker container installation

### 1. Clone or download this repository
```
git clone https://github.com/modellbibliotek/klinfys-register.git
```

### 2. Go to the ehrbase directory
```
cd klinfys-register/ehrbase
```

### 3. Compose the multi-application container (App + DB)
```
docker-compose up
```

### 4. The container should now be running, test this by running ```docker ps```.

```
CONTAINER ID   IMAGE                             COMMAND                  CREATED         STATUS          PORTS                                       NAMES
b3f1f51636ee   ehrbase/ehrbase-postgres:latest   "docker-entrypoint.s…"   6 minutes ago   Up 8 seconds    0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   ehrbase_ehrdb_1
0dd30f478f11   ehrbase/ehrbase:next              "/bin/sh -c ./docker…"   6 minutes ago   Up 11 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   ehrbase_ehrbase_1
```
