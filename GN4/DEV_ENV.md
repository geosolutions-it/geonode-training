# Preparation of the Dev Environment

## Start GeoNode in DEV Mode
In this section we will learn how to run GeoNode in _development mode_; this particular way to run GeoNode will allow us to view and debug any change to the code at runtime, without the need to restart the services.

Notice that we will still need the following services to run on the system:

 - **PostgreSQL service w/ Portgis extensions**; we will learn how to link Dev GeoNode to an existing database and how to initialize a new one.
 - **Apache Tomcat9 with GeoServer**; we will still need a running GeoServer instance to be able to manage the geospatial datasets.

### Stop GeoNode Services
- Stop `NGINX` and `UWSGI` services

```shell
sudo systemctl stop nginx

sudo systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Tue 2021-09-07 15:56:19 BST; 19s ago
       Docs: man:nginx(8)
    Process: 633 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 679 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 4078 ExecStop=/sbin/start-stop-daemon --quiet --stop --retry QUIT/5 --pidfile /run/nginx.pid (code=exited, status=0/SUCCESS)
   Main PID: 683 (code=exited, status=0/SUCCESS)

Sep 07 15:49:45 geonodevm-3 systemd[1]: Starting A high performance web server and a reverse proxy server...
Sep 07 15:49:46 geonodevm-3 systemd[1]: Started A high performance web server and a reverse proxy server.
Sep 07 15:56:19 geonodevm-3 systemd[1]: Stopping A high performance web server and a reverse proxy server...
Sep 07 15:56:19 geonodevm-3 systemd[1]: nginx.service: Succeeded.
Sep 07 15:56:19 geonodevm-3 systemd[1]: Stopped A high performance web server and a reverse proxy server.
```

```shell
sudo systemctl stop uwsgi

sudo systemctl status uwsgi
● uwsgi.service - LSB: Start/stop uWSGI server instance(s)
     Loaded: loaded (/etc/init.d/uwsgi; generated)
     Active: failed (Result: exit-code) since Tue 2021-09-07 15:49:51 BST; 7min ago
       Docs: man:systemd-sysv-generator(8)
    Process: 714 ExecStart=/etc/init.d/uwsgi start (code=exited, status=1/FAILURE)
      Tasks: 9 (limit: 4650)
     Memory: 628.9M
     CGroup: /system.slice/uwsgi.service
             ├─ 909 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
             ├─1260 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
             ├─1261 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
             ├─1262 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
             ├─1263 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
             ├─1264 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
             ├─1266 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
             ├─1270 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
             └─1274 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log

Sep 07 15:49:51 geonodevm-3 systemd[1]: uwsgi.service: Failed with result 'exit-code'.
Sep 07 15:49:51 geonodevm-3 systemd[1]: Failed to start LSB: Start/stop uWSGI server instance(s).
Sep 07 15:50:21 geonodevm-3 uwsgi[1252]: DIGEST-MD5 common mech free
Sep 07 15:50:30 geonodevm-3 uwsgi[1254]: DIGEST-MD5 common mech free
Sep 07 15:50:31 geonodevm-3 uwsgi[1253]: DIGEST-MD5 common mech free
Sep 07 15:50:32 geonodevm-3 uwsgi[1256]: DIGEST-MD5 common mech free
Sep 07 15:50:33 geonodevm-3 uwsgi[1255]: DIGEST-MD5 common mech free
Sep 07 15:50:33 geonodevm-3 uwsgi[1257]: DIGEST-MD5 common mech free
Sep 07 15:50:33 geonodevm-3 uwsgi[1258]: DIGEST-MD5 common mech free
Sep 07 15:50:34 geonodevm-3 uwsgi[1259]: DIGEST-MD5 common mech free

sudo ps aux | grep uwsgi
www-data     909  2.6  4.6 633312 185296 ?       S    15:49   0:12 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
www-data    1260  0.0  3.2 636216 132544 ?       S    15:50   0:00 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
www-data    1261  0.0  3.2 633956 129732 ?       S    15:50   0:00 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
www-data    1262  0.0  3.3 636124 133028 ?       S    15:50   0:00 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
www-data    1263  0.2  3.4 641636 139464 ?       S    15:50   0:01 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
www-data    1264  0.1  3.4 639988 139432 ?       S    15:50   0:00 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
www-data    1266  0.2  3.4 641840 138692 ?       S    15:50   0:00 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
www-data    1270  0.2  3.4 642644 140540 ?       S    15:50   0:01 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
www-data    1274  0.2  3.7 645728 150104 ?       S    15:50   0:01 /usr/bin/uwsgi --ini /usr/share/uwsgi/conf/default.ini --ini /etc/uwsgi/apps-enabled/geonode.ini --daemonize /var/log/uwsgi/app/geonode.log
geonode+    4123  0.0  0.0  17544   724 pts/0    R+   15:57   0:00 grep --color=auto uwsgi

sudo pkill -9 uwsgi

sudo ps aux | grep uwsgi
geonode+    4131  0.0  0.0  17544   660 pts/0    S+   15:57   0:00 grep --color=auto uwsgi
```

- Switch to `geonode` virtual env

```shell
workon geonode
cd /opt/geonode
```

### Prepare the .env_dev Variables
- Adjust the `.end_dev` file in order to match our current configuration, make sure `SITEURL`, DB connection settings and `GEOSERVER_*` URLs and connection params are correct

```shell
vim .env_dev
```

```ini
...
# #################
# backend
# #################
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
GEONODE_DATABASE=geonode
GEONODE_DATABASE_PASSWORD=geonode
GEONODE_GEODATABASE=geonode_data
GEONODE_GEODATABASE_PASSWORD=geonode
GEONODE_DATABASE_SCHEMA=public
GEONODE_GEODATABASE_SCHEMA=public
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_URL=postgis://geonode:geonode@localhost:5432/geonode
GEODATABASE_URL=postgis://geonode:geonode@localhost:5432/geonode_data
GEONODE_DB_CONN_MAX_AGE=0
GEONODE_DB_CONN_TOUT=5
DEFAULT_BACKEND_DATASTORE=datastore
...
SITEURL=http://localhost:8000/
...
# #################
# geoserver
# #################
GEOSERVER_WEB_UI_LOCATION=http://localhost:8080/geoserver/
GEOSERVER_PUBLIC_LOCATION=http://localhost:8080/geoserver/
GEOSERVER_LOCATION=http://localhost:8080/geoserver/
GEOSERVER_ADMIN_USER=admin
GEOSERVER_ADMIN_PASSWORD=geoserver
```

### Make sure the DB and GeoNode are aligned

- Align the migrations and static/media folders

```shell
./paver_dev.sh sync
```

- Align the internal URLs and Metadata links

```shell
# The order is important! Those are regex expressions and will be executed one after the other...

# Fix GeoServer URLs first
./manage_dev.sh migrate_baseurl --source-address=http://localhost/geoserver --target-address=http://localhost:8080/geoserver

# Fix GeoNode URLs
./manage_dev.sh migrate_baseurl --source-address=http://localhost/ --target-address=http://localhost:8000/

# Align the Metadata links
./manage_dev.sh set_all_layers_metadata -d
```

### Make sure the GeoServer OAuth plugin is correctly configured
- Adjust the GeoServer `PROXY_BASE_URL`
  1. Login as `admin/geoserver` by using the `form login`
  2. Update the `Global` settings of GeoServer


![image](https://user-images.githubusercontent.com/1278021/132375154-5b5f9eae-d07b-4147-83e3-f74f1386a240.png)

- Adjust the `REST Role Service`

![image](https://user-images.githubusercontent.com/1278021/132375708-a5ecfedf-eca7-4cdc-934c-592e7726504c.png)

- Adjust the `OAuth2 Security Filter`

![image](https://user-images.githubusercontent.com/1278021/132375932-9ec8b03d-ef30-4613-89ee-d72f245d4c90.png)

![image](https://user-images.githubusercontent.com/1278021/132376323-d9d63007-8775-4777-b04d-f0cba837c2b6.png)

- Test the GeoServer `logout/login` with GeoNode

### Let's Start GeoNode
- Let's refresh the layers thumbnails and verify the settings are correct

```shell
./manage_dev.sh sync_geonode_layers --updatethumbnails

Syncing layer 1/11: a__13tde815295_200803_0x6000m_cl
Regenerating thumbnails...
Syncing layer 2/11: Air_Runways
Regenerating thumbnails...
Syncing layer 3/11: BoulderCityLimits
Regenerating thumbnails...
Syncing layer 4/11: Buildings050714
Regenerating thumbnails...
Syncing layer 5/11: Mainrd
Regenerating thumbnails...
Syncing layer 6/11: Parcels
Regenerating thumbnails...
Syncing layer 7/11: pointlm
Regenerating thumbnails...
Syncing layer 8/11: srtm_boulder
Regenerating thumbnails...
Syncing layer 9/11: Streets
Regenerating thumbnails...
Syncing layer 10/11: Trails
Regenerating thumbnails...
Syncing layer 11/11: Wetlands_regulatory_area
Regenerating thumbnails...
There are 0 layers which could not be updated because of errors
```

- Start GeoNode in `development mode`

```shell
./paver_dev.sh start_django

---> pavement.start_django
 python -W ignore manage.py runserver 0.0.0.0:8000 &
Performing system checks...

System check identified some issues:

WARNINGS:
?: (urls.W005) URL namespace 'rest_framework' isn't unique. You may not be able to reverse all URLs in this namespace

System check identified 1 issue (5 silenced).
September 07, 2021 - 16:07:13
Django version 2.2.20, using settings 'geonode.settings'
Starting development server at http://0.0.0.0:8000/
Quit the server with CONTROL-C.
```

- Connect to `http://localhost:8000` and verify GeoNode has started correctly

#### [Next Section: Programmatically Customize the Look-and-Feel](LANDF_SIMPLE.md)
