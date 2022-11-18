# Code DX cheatsheet

## Installation (docker, with SSL)

* Generate key/cert

```bash
openssl req -new -newkey rsa:4096 -days 3650 -nodes -x509 -subj "/C=US/ST=New York/L=Northport/O=Code Dx/CN=localhost" -keyout ./ssl.key -out ./ssl.crt
```

* Update docker-compose.yml adding ssl.key, ssl.crt, and server.xml and ports

```yaml
volumes:
    - codedx-appdata:/opt/codedx
    - /path/to/ssl.crt:/usr/local/tomcat/conf/ssl.crt
    - /path/to/ssl.key:/usr/local/tomcat/conf/ssl.key
    - /path/to/server.xml:/usr/local/tomcat/conf/server.xml
ports:
    - 8443:8443
depends_on:
    - codedx-db
```

* Start the app

```bash
docker compose -f docker-compose.yml up
```

## Change log level

* Check the current log level (default is INFO)
```bash
docker exec 955fe4713473 grep "root level" /opt/codedx/logback.xml
```

* Change the log level to TRACE
```bash
* edit logback.xml
docker exec <Tomcat_container_id> sed -i 's/root level\=\"INFO\"/root level\=\"TRACE\"/g' /opt/codedx/logback.xml

* restart the Tomcat container
docker restart <Tomcat_container_id>
```


* Revert changes
```bash
* edit logback.xml
docker exec <Tomcat_container_id> sed -i 's/root level\=\"TRACE\"/root level\=\"INFO\"/g' /opt/codedx/logback.xml

* restart the Tomcat container
docker restart <Tomcat_container_id>
```
