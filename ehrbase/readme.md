- [x] Create a composition example
- [x] AQL Example
- [x] FHIR Bridge example

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
CONTAINER ID   IMAGE                             COMMAND                  CREATED         STATUS          PORTS                                       NAMES
b3f1f51636ee   ehrbase/ehrbase-postgres:latest   "docker-entrypoint.s…"   6 minutes ago   Up 8 seconds    0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   ehrbase_ehrdb_1
0dd30f478f11   ehrbase/ehrbase:next              "/bin/sh -c ./docker…"   6 minutes ago   Up 11 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   ehrbase_ehrbase_1
```
## REST API

### Prerequisites
- Postman must be installed on your computer

### Import the [Postman collection and environment](https://github.com/modellbibliotek/klinfys-register/tree/master/ehrbase/Postman)
![Postman Screenshot](https://github.com/modellbibliotek/klinfys-register/blob/master/ehrbase/Postman.png)
