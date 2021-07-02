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
# REST API

## Prerequisites
- Postman must be installed on your computer
- Postman collection
### Import the Postman collection

## Postman collection requests
### Create a new EHR
send the saved POST request _ehr/Create a new EHR_ from the imported Postman collection. If successful, copy the ehr_id/value and store it in your environment variable _ehr_id_
```json
...
"ehr_id": {
    "_type": "HIER_OBJECT_ID",
    "value": "8d589fbf-a879-4e94-b1e9-52f22eab9637"
},
...
```
### Fetch an EHR by ehr id
Send the saved GET request _ehr/Fetch EHR by ehr_id_. The response should look like the previous one
```json
...
{
  "system_id": {
      "_type": "HIER_OBJECT_ID",
      "value": "4de35dbd-774c-4de1-8b24-6cc6dd129068"
  },
  ...
}
```
### Upload a new template
Send the saved POST request _definition/Upload a template_. The body XML is taken from an OPT file. The response body should be similar to the request body.

### List all uploaded templates
Send the saved GET requst _definition/List templates_

Store the value for the key _template_id_ from the response as your environment variable _template_id_

### Retrieve a specified template

Send the saved GET request _definition/Retrieve a template_

