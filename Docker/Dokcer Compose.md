#docker #dokcer-compose

# what is DockerCompose 
**Docker Compose** is a tool for defining and running multi-container Docker applications. It allows you to define the services, networks, and volumes needed for your application in a single file called a "docker-compose.yml" file, and then **start and stop the entire application using a single command**. With Docker Compose, you can easily manage the configuration, scaling, and networking of your application, as well as spin up and down entire applications as needed. This makes it ideal for development, testing, and **production environments**, where you need to quickly spin up and down complex applications without manual intervention.
```yaml
version: '3.7'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: password

```

Compose **preserves** all volumes used by your services. When docker-compose runs, if it finds any containers from previous runs, it <ins>copies the volumes from the old container to the new container</ins>. This process ensures that any data you’ve created in volumes isn’t lost.

Compose **caches** the configuration used to create a container.

You can control the order of service startup and shutdown with the ==depends_on== option,Compose always starts and stops containers in dependency order, where dependencies are determined by <ins>depends\_on, links, volumes\_from, and network_mode: "service:...</ins>".

* * *
## Changes for production:
Removing any volume bindings for application code, so that code stays inside the container and can’t be changed from outside

- ==Binding to different ports on the host==
- ==Setting environment variables differently, such as reducing the verbosity of logging, or to specify settings for external services such as an email server==
- Specifying a restart policy like restart: **always** to avoid downtime
- Adding extra services such as a **log aggregator**

* * *
## Deploying changes:
When you make changes to your app code, remember to rebuild your image and **recreate** your app’s containers.

```bash
docker-compose build web

docker-compose up --no-deps -d web
```

This first rebuilds the image for web and then stop, destroy, and recreate just the web service. The `--no-deps` flag prevents Compose from also recreating any services which web depends on.
* * *
Compose supports declaring default environment variables in an environment file named .**env** placed in the project directory.

The following syntax rules apply to the .env file:
- Compose expects each line in an env file to be in **VAR=VAL** format.
- Lines beginning with **#** are processed as **comments** and ignored.
- Blank lines are ignored.
- <ins>There is no special handling of quotation marks. This means that they are part of the VAL.</ins>
```.env
POSTGRES_USER=mydbuser
POSTGRES_PASSWORD=secretpassword
POSTGRES_DB=mydb
PGDATA=/var/lib/postgresql/data/pgdata
```


By default Compose sets up a **single network for your app**. Each container for a service joins the default network and is both reachable by other containers on that network, and **discoverable by them at a hostname identical to the container name**.

## Links
<u>Links allow you to define extra aliases by which a service is reachable from another service. They are not required to enable services to communicate</u> - **by default**, any service can reach any other service at that **service’s name**.

```yaml
links:
  - "db:database"
```

## Use a pre-existing network:
If you want your containers to join a **pre-existing network**, use the `external` option.
```yaml
services: 
# ...
networks: 
   default: 
       external: true
       name: my-pre-existing-network
```

* * *
## Multiple Compose files
Using multiple Compose files enables you to customize a Compose application for different environments or different workflows.

By default, Compose reads two files, a **docker-compose.yml** and an optional docker-**compose.override.yml** file. By convention, the docker-compose.yml contains your base configuration. The override file, as its name implies, can contain configuration overrides for existing services or entirely new services.
* * *
## Some references in compose:
### **<font color="#ffff00">context</font>**:
Either a path to a directory <u>containing a Dockerfile</u>, or a url to a git repository.

### **<font color="#ffff00">command</font>:**
Override the default command.
```yaml
command: bundle exec thin -p 3000
```

### **<font color="#ffff00">container_name</font>:**
Specify a custom container name, rather than a generated default name.
```yaml
container_name: my-web-container
```

### **<font color="#ffff00">entrypoint</font>**:
Override the default entrypoint.
entrypoint: /code/entrypoint.sh 

### **<font color="#ffff00">env_file</font>:**
Add environment variables from a file. Can be a single value or a list.

### **<font color="#ffff00">healthcheck</font>:**
Configure a check that’s run to determine whether or not containers for this service are “healthy”.
```yaml
healthcheck:
    test: ["CMD", "curl", "-f", "http://localhost"\]
    interval: 1m30s
    timeout: 10s
    retries: 3
    start_period: 40s
```

### **<font color="#ffff00">logging</font>:**
Logging configuration for the service.
```yaml
logging:
 	driver: syslog
 	options:
 	 	syslog-address: "tcp://192.168.0.42:123"
```

**The driver name specifies a logging drive**r for the service’s containers, as with the --log-driver option for docker run.

The default value is **json-file**.

- driver: "json-file"
- driver: "syslog"
- driver: "none"

==Only the json-file and journald drivers make the logs available directly from docker-compose up and docker-compose logs==

The default driver json-file, has <ins>options to limit the amount of logs stored</ins>. To do this, use a key-value pair for <ins>maximum storage size and maximum number of files</ins>:

```yaml
options: 
    max-size: "200k"
    max-file: "10"
```

### <font color="#ffff00">restart</font>
`no` is the default restart policy, and it does not restart a container under any circumstance. When `always` is specified, the container always restarts. The `on-failure` policy restarts a container if the exit code indicates an on-failure error. `unless-stopped` always restarts a container, except when the container is stopped (manually or otherwise).

```
restart: "no"
restart: always
restart: on-failure
restart: unless-stopped 
```