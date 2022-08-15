
## Components

### 1. iSantePlus EMR
### Links
https://github.com/IsantePlus/openmrs-distro-isanteplus
https://github.com/IsantePlus/docker-isanteplus-server

### 2. OpenCR
https://github.com/intrahealth/client-registry

### 3. OpenHIM
http://openhim.org/docs/installation/docker

### 4. HAPI JPA Server
https://github.com/hapifhir/hapi-fhir-jpaserver-starter#deploy-with-docker-compose
https://hapifhir.io/hapi-fhir/docs/server_jpa/get_started.html



## Installation
### 1. Install Docker

**Docker Engine:**

https://docs.docker.com/compose/install/

**Docker Compose:**

https://docs.docker.com/compose/install/


### 2. Clone the  Repository

```sh
git clone https://github.com/I-TECH-UW/sedish-haiti.org.git
```

### 3. Port-based Setup


**a) Pull all containers**  

```sh
sudo docker-compose -f docker-compose.ports.yml pull
```
**b) Start up Core Containers** 

Generate self-signed certs:  
```sh
sudo docker-compose -f docker-compose.ports.yml up certgen
```

```sh
sudo docker-compose -f docker-compose.ports.yml up -d nginx openhim-core openhim-console mongo-db
```

**c) Load Default OpenHIM Config**
First, make sure to choose and set a desired admin PW for OpenHIM that you'll use in all of the mediator configuration. 

You can set this Password with the "ADMIN_PW" env setting. 

```sh
sudo docker-compose -f docker-compose.ports.yml up openhim-config
```

**d) Access the OpenHIM Console**  
You should now be able to access the OpenHIM console at https://localhost, or whatever IP address the server is running on. The OpenHIM console runs on ports 80 and 443. 

*Note: If you are using Chrome and get a certificate error, you can type `thisisunsafe` after clicking anywhere on the page to be able to proceed.*

Make sure that the console is pointint to the correct `openhim-core` container. You should be able to access that container using `<your-ip-address>:8080/heartbeat`. You can configure this connection in `configs/openhim-console/ports.json`. 

**e) Modify OpenHIM settings as desired**   
Log in to the console, and set the admin password to `openhim` (for development), or the password of your choice if you haven't done so earlier automatically.

You can also set up Clients and Roles for the following systems:
- postman for testing
- each isanteplus instance that will connect to the HIE
- the Shared Health Record

*Note: any changes to the OpenHIM console container might not show up until you disable/clear the browser cache. You can also disable the cache by opening Chrome dev tools with F12 and selecting the `disable cache` checkbox*

**f) Start up Support Containers**  
```sh
sudo docker-compose -f docker-compose.ports.yml up -d shr-fhir opencr-fhir opencr-es kafka zookeeper
```

#### Start up iSantePlus
```sh
sudo docker-compose -f docker-compose.ports.yml up -d isanteplus-mysql isanteplus
```
#### Start up SHR Streaming Pipeline
```sh
sudo docker-compose -f docker-compose.ports.yml up -d streaming-pipeline
```

## 4. Testing and Validation
Setup a Postman environment and run tests from this workspace: https://www.postman.com/itechuw/workspace/haiti-sedish



