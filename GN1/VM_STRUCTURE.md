# GeoNode Virtual Machine Structure
The Virtual Machine is based on a distribution of:

 - `Ubuntu 20.04.2 LTS (codename Focal) Desktop 64-bit`

It has been installed with a minimal set of dependencies, except the [ones required by the GeoNode installation](https://docs.geonode.org/en/master/install/advanced/core/index.html#install-the-dependencies).

## System and Application Users' Credentials

| Username        | Password      | Type                                 |
|-----------------|---------------|--------------------------------------|
| `geonode-vm-321`| `geonode`     | `System user with root power`        |
| `admin`         | `admin`       | `GeoNode default superuser`          |
| `admin`         | `geoserver`   | `GeoServer default admin (internal)` |
| `postgres`      |               | `Postgresql 13 default superuser`    |
| `geonode`       | `geonode`     | `Postgresql 13 geonode DB owner`     |
| `geonode`       | `geonode`     | `Postgresql 13 geonode_data DB owner`|

## Changing Screen Resolution and Keyboard Type

### Screen Resolution
- You can change the screen resolution by clicking with the `mouse right button` to the desktop and selecting the `Display Settings menu`
    ![image](https://user-images.githubusercontent.com/1278021/125261119-43638480-e301-11eb-8770-2b98c3da39e0.png)

- Select the desired resolution from the dropdown selector and then click on `Apply`
    ![image](https://user-images.githubusercontent.com/1278021/125261539-a523ee80-e301-11eb-8e8d-d8c4cb58d417.png)

### Keyboard Layout
 - You can change the screen resolution by clicking with the `mouse right button` to the desktop and selecting the `Settings menu`
     ![image](https://user-images.githubusercontent.com/1278021/125261761-ddc3c800-e301-11eb-9c67-c5e9b716765c.png)

 - Clck on the `plus button`
     ![image](https://user-images.githubusercontent.com/1278021/125261913-01870e00-e302-11eb-95a9-9ee9cf52606e.png)

 - Click on the vertical points
     ![image](https://user-images.githubusercontent.com/1278021/125262094-2c716200-e302-11eb-87e0-e9f9b2c3ae4d.png)

 - If you don't find the desired language, click on the `Other` button
     ![image](https://user-images.githubusercontent.com/1278021/125262456-79edcf00-e302-11eb-8f9e-d8c6adfbeb54.png)

 - Search and select the desired language from the list and click on the `Add` button
     ![image](https://user-images.githubusercontent.com/1278021/125262544-8f62f900-e302-11eb-86e6-29514c594a38.png)

 - Drag & drop the desired language to the top and delete the others
     ![image](https://user-images.githubusercontent.com/1278021/125262906-dbae3900-e302-11eb-8a47-af4106a5c8a5.png)

## System Services and Log files

### Postgresql 13 DBMS
This GeoNode installation relies on a DB hosted by an instance of [Postgresql 13](https://www.postgresql.org/about/news/postgresql-13-released-2077/) service, with the [PostGIS Extensions](https://postgis.net/install/).

#### Start and Stop the Service
- In order to check the service status run:

  `sudo systemctl status postgresql`
  
  The system will print something like:
  ```shell
  ● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
     Active: active (exited) since Mon 2021-07-12 09:30:39 BST; 2s ago
    Process: 5458 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 5458 (code=exited, status=0/SUCCESS)
  ```
- Stop the service by running:

  `sudo systemctl stop postgresql`

- Start the service by running:

  `sudo systemctl start postgresql`

- Check and follow the service logs by running:

  `sudo tail -500f /var/log/postgresql/postgresql-13-main.log`

### GeoServer 2.18.3
The geospatial server backend is provided by [GeoServer 2.18.3](http://geoserver.org/announcements/2021/04/23/geoserver-2.18.3-released.html) hosted by an instance of the [Apache Tomcat 9.0.48](https://tomcat.apache.org/download-90.cgi) servlet application provider.

You can access the service interface by pointing the browser to:

`http://localhost/geoserver`

#### Start and Stop the Service
- In order to check the service status run:

  `sudo systemctl status tomcat9`
  
  The system will print something like:
  ```shell
  ● tomcat9.service - Apache Tomcat Server
     Loaded: loaded (/lib/systemd/system/tomcat9.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2021-07-12 09:41:44 BST; 2s ago
    Process: 6444 ExecStart=/opt/tomcat/latest/bin/startup.sh (code=exited, status=0/SUCCESS)
   Main PID: 6454 (java)
      Tasks: 17 (limit: 4652)
     Memory: 119.3M
     CGroup: /system.slice/tomcat9.service
             └─6454 /usr/lib/jvm/java-8-openjdk-amd64/jre//bin/java -Djava.util.logging.config.file=/opt/tomcat/latest/conf/logging.properties -Djava.util.logging.mana>

  Jul 12 09:41:44 geonodevm-3 systemd[1]: Starting Apache Tomcat Server...
  Jul 12 09:41:44 geonodevm-3 startup.sh[6444]: Existing PID file found during start.
  Jul 12 09:41:44 geonodevm-3 startup.sh[6444]: Removing/clearing stale PID file.
  Jul 12 09:41:44 geonodevm-3 startup.sh[6444]: Tomcat started.
  Jul 12 09:41:44 geonodevm-3 systemd[1]: Started Apache Tomcat Server.
  ```
- Stop the service by running:

  `sudo systemctl stop tomcat9`

- Start the service by running:

  `sudo systemctl start tomcat9`

- Check and follow the service logs by running:

  `sudo tail -500f /opt/data/geoserver_logs/geoserver.log`

#### GeoServer DATA_DIR and JVM Options

The defailt GeoServer `JVM Options` (heap memory, logs and data dir locations, ...) can be set by editing the following file:

`sudo vim /opt/tomcat/latest/bin/setenv.sh`

By default those options are set as follows:

```properties
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/
GEOSERVER_DATA_DIR="/opt/data/geoserver_data"
GEOSERVER_LOG_LOCATION="/opt/data/geoserver_logs/geoserver.log"
GEOWEBCACHE_CACHE_DIR="/opt/data/gwc_cache_dir"
GEOFENCE_DIR="$GEOSERVER_DATA_DIR/geofence"
TIMEZONE="UTC"
JAVA_OPTS="-server -Djava.awt.headless=true -Dorg.geotools.shapefile.datetime=false -XX:+UseParallelGC -XX:ParallelGCThreads=4 -Dfile.encoding=UTF8 -Duser.timezone=$TIMEZONE -Xms512m -Xmx2048m -Djavax.servlet.request.encoding=UTF-8 -Djavax.servlet.response.encoding=UTF-8 -DGEOSERVER_CSRF_DISABLED=true -DPRINT_BASE_URL=http://localhost:8080/geoserver/pdf -DGEOSERVER_DATA_DIR=$GEOSERVER_DATA_DIR -Dgeofence.dir=$GEOFENCE_DIR -DGEOSERVER_LOG_LOCATION=$GEOSERVER_LOG_LOCATION -DGEOWEBCACHE_CACHE_DIR=$GEOWEBCACHE_CACHE_DIR"
```

The default `GEOSERVER_DATA_DIR`, containing the GeoServer catalog, is set to:

```properties
GEOSERVER_DATA_DIR="/opt/data/geoserver_data"
```

### NGINX 1.18.0 HTTPD Server
All the HTTP services are provided through an instance of the [NGINX 1.18.0](https://www.nginx.com/) HTTPD Server.

This service allows to proxy every HTTP based application through the `http://localhost` virtual host.

#### Start and Stop the Service
- In order to check the service status run:

  `sudo systemctl status nginx`
  
  The system will print something like:
  ```shell
  ● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2021-07-12 09:05:30 BST; 45min ago
       Docs: man:nginx(8)
    Process: 636 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 693 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 705 (nginx)
      Tasks: 3 (limit: 4652)
     Memory: 11.2M
     CGroup: /system.slice/nginx.service
             ├─705 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
             ├─710 nginx: worker process
             └─711 nginx: worker process

  Jul 12 09:05:29 geonodevm-3 systemd[1]: Starting A high performance web server and a reverse proxy server...
  Jul 12 09:05:30 geonodevm-3 systemd[1]: Started A high performance web server and a reverse proxy server.
  ```

- Stop the service by running:

  `sudo systemctl stop nginx`

- Start the service by running:

  `sudo systemctl start nginx`

- Check and follow the service logs by running:

  `sudo tail -500f /var/log/nginx/access.log`
  `sudo tail -500f /var/log/nginx/error.log`

#### NGINX localhost configuration files

- The main configuration file can be edited by running the following command:

  `sudo vim /etc/nginx/nginx.conf`

- The GeoNode/GeoServer configuration file can be edited by running the following command:

  `sudo vim /etc/nginx/sites-enabled/geonode`


#### [Next Section: Accounts and User Profile](GN_ACCOUNTS_PROFILES.md)
