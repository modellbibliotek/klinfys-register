- [x] Composition example
- [x] AQL Example
- [x] FHIR Bridge example
- [ ] HL7 v2 -> FHIR Bridge example 

# Docker EHRBase App + EHRBase DB + FHIR Bridge

## Prerequisites
- Docker must be installed and running on your computer
> The Docker proxy setting must be configured if your network has internet access through a proxy

## Docker container installation

#### 1. Clone/download and compose this repository
```bash
git clone https://github.com/modellbibliotek/klinfys-register.git
cd klinfys-register/ehrbase
docker-compose up
```

#### 2. The container should now be running, test this by running ```docker ps```.

```bash
CONTAINER ID   IMAGE                             COMMAND                  CREATED       STATUS       PORTS                                       NAMES
bd1edb8daf68   ehrbase/fhir-bridge:next          "/cnb/process/web"       3 hours ago   Up 3 hours   0.0.0.0:8888->8888/tcp, :::8888->8888/tcp   docker_fhir-bridge_1
8da79eb3ceb2   ehrbase/ehrbase:next              "/bin/sh -c ./docker…"   3 hours ago   Up 3 hours   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   docker_ehrbase_1
f1164611dfc1   ehrbase/ehrbase-postgres:latest   "docker-entrypoint.s…"   3 hours ago   Up 3 hours   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   docker_ehrbase-db_1
```
## REST API

### Prerequisites
- Postman must be installed on your computer

### Import the [Postman collection and environment](https://github.com/modellbibliotek/klinfys-register/tree/master/ehrbase/Postman)
![Postman Screenshot](https://github.com/modellbibliotek/klinfys-register/blob/master/ehrbase/Postman.png)
