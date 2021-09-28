# Link GeoNode to a geonode-project instance
A `geonode-project` is basically a Django project **template** allowing us to create a brand new web project which _has GeoNode core as a dependency_.

In simple words, through this template project we are able to customize somehow our GeoNode instance without actually touching the GeoNode core code.

Thanks to the prower of Django, we are able to override **templates**, **views** and, withing certain limits, the **models**.

In this section we will learn how to:

 - Create a new `virtualenv` for our `geonode-project` instance
 - Initialize a new `geonode-project` instance called `my_geonode`
 - Modify the look-and-feel from the `my_geonode` instance

## Create `my_geonode` Django Project
- First of all we need to create a new VirtualEnv

```shell
# Let's recognize where the Python 3.8 is located
which python3.8
/usr/bin/python3.8
```
```shell
# Let's create the virtualenv based on Python 3.8
mkvirtualenv -p /usr/bin/python3.8 my_geonode
created virtual environment CPython3.8.10.final.0-64 in 385ms
  creator CPython3Posix(dest=/home/geonode-vm-321/.virtualenvs/my_geonode, clear=False, global=False)
  seeder FromAppData(download=False, pip=latest, setuptools=latest, wheel=latest, pkg_resources=latest, via=copy, app_data_dir=/home/geonode-vm-321/.local/share/virtualenv/seed-app-data/v1.0.1.debian.1)
  activators BashActivator,CShellActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator
virtualenvwrapper.user_scripts creating /home/geonode-vm-321/.virtualenvs/my_geonode/bin/predeactivate
virtualenvwrapper.user_scripts creating /home/geonode-vm-321/.virtualenvs/my_geonode/bin/postdeactivate
virtualenvwrapper.user_scripts creating /home/geonode-vm-321/.virtualenvs/my_geonode/bin/preactivate
virtualenvwrapper.user_scripts creating /home/geonode-vm-321/.virtualenvs/my_geonode/bin/postactivate
virtualenvwrapper.user_scripts creating /home/geonode-vm-321/.virtualenvs/my_geonode/bin/get_env_details
(my_geonode) geonode-vm-321@geonodevm-3:~$
```
```shell
# Let's install the correct version of Django
pip install Django==2.2.20
Collecting Django==2.2.20
  Downloading Django-2.2.20-py3-none-any.whl (7.5 MB)
     |████████████████████████████████| 7.5 MB 3.5 MB/s 
Collecting sqlparse>=0.2.2
  Downloading sqlparse-0.4.1-py3-none-any.whl (42 kB)
     |████████████████████████████████| 42 kB 508 kB/s 
Collecting pytz
  Downloading pytz-2021.1-py2.py3-none-any.whl (510 kB)
     |████████████████████████████████| 510 kB 3.3 MB/s 
Installing collected packages: sqlparse, pytz, Django
Successfully installed Django-2.2.20 pytz-2021.1 sqlparse-0.4.1
```

- The new VirtialEnv called `my_geonode` is now ready to be used.
- Let's create the `my_django` project by using the `geonode-project` as a template

```shell
# Let's create the target folder first, with the right permissions
cd /opt
sudo mkdir geonode-project
sudo chown -Rf geonode-vm-321: geonode-project/
cd geonode-project/
```
- Let's create the instance by cloning the geonode-project template
- First of all we need to get the correct link to the `geonode-project` template
- Go to `https://github.com/GeoNode/geonode-project` and switch to the `3.2.x` branch

![image](https://user-images.githubusercontent.com/1278021/132707758-69073cd2-cadc-49e9-934e-407166d46f56.png)

- Copy the `zip file` link address

![image](https://user-images.githubusercontent.com/1278021/132708095-6fc12adc-f2c7-4697-b30c-9a2107f1dd60.png)

- Let's finally create the project


```shell
# Let's create the instance by cloning the geonode-project template
django-admin startproject --template=https://github.com/GeoNode/geonode-project/archive/refs/heads/3.2.x.zip -e py,sh,md,rst,json,yml,ini,env,sample,properties -n monitoring-cron -n Dockerfile my_geonode
```

The previous command line should create a new folder called `my_geonode` containing a Django project instance

```shell
/opt/geonode-project/my_geonode$ ll
total 220
drwxrwxr-x 7 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 ./
drwxr-xr-x 3 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 ../
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321  1261 Sep  9 15:43 celery-cmd*
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   524 Sep  9 15:43 celery.sh
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   644 Sep  9 15:43 dev_config.yml
drwxrwxr-x 6 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 docker/
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321    98 Sep  9 15:43 docker-build.sh*
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  6887 Sep  9 15:43 docker-compose.development.yml
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   685 Sep  9 15:43 docker-compose.override.yml
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  4720 Sep  9 15:43 docker-compose.yml
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  3000 Sep  9 15:43 Dockerfile
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321   177 Sep  9 15:43 docker-purge.sh*
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321  3121 Sep  9 15:43 entrypoint.sh*
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  7138 Sep  9 15:43 .env
drwxrwxr-x 2 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 fixtures/
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   829 Sep  9 15:43 .gitignore
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   506 Sep  9 15:43 jetty-runner.xml
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   559 Sep  9 15:43 Makefile
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321    52 Sep  9 15:43 manage_dev.sh.sample
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321  1092 Sep  9 15:43 manage.py*
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321    77 Sep  9 15:43 manage.sh*
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   256 Sep  9 15:43 monitoring-cron
drwxrwxr-x 5 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 my_geonode/           <------- Django main app folder
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  3308 Sep  9 15:43 .override_dev_env.sample
drwxrwxr-x 3 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 package/
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 41964 Sep  9 15:43 pavement.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321    40 Sep  9 15:43 paver_dev.sh.sample
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321    31 Sep  9 15:43 paver.sh*
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  1566 Sep  9 15:43 playbook.yml
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  6173 Sep  9 15:43 README.md
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321    80 Sep  9 15:43 requirements.txt       <------- Django dependencies
drwxrwxr-x 3 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 scripts/
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  1648 Sep  9 15:43 setup.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 22993 Sep  9 15:43 tasks.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  2445 Sep  9 15:43 uwsgi.ini
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321   691 Sep  9 15:43 wait-for-databases.sh*
```

By taking a look into the `/opt/geonode-project/my_geonode/my_geonode` you should be able to recognize a standard Django app structure

```shell
drwxrwxr-x 5 geonode-vm-321 geonode-vm-321 4096 Sep  9 15:43 ./
drwxrwxr-x 7 geonode-vm-321 geonode-vm-321 4096 Sep  9 15:43 ../
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1279 Sep  9 15:43 apps.py
drwxrwxr-x 2 geonode-vm-321 geonode-vm-321 4096 Sep  9 15:43 br/
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1356 Sep  9 15:43 celeryapp.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1042 Sep  9 15:43 __init__.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 5000 Sep  9 15:43 settings.py   <------- Django settings
drwxrwxr-x 6 geonode-vm-321 geonode-vm-321 4096 Sep  9 15:43 static/
drwxrwxr-x 2 geonode-vm-321 geonode-vm-321 4096 Sep  9 15:43 templates/    <------- HTML templates
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1234 Sep  9 15:43 urls.py       <------- main URL patterns
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1779 Sep  9 15:43 version.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1980 Sep  9 15:43 wsgi.py
```

Some files are still missing, like the **views**, **models** and **migrations**, since they are not needed by default.

## Start `my_geonode` in DEV Mode
- We need to install the dependencies first

```shell
# Let's install the GeoNode dependencies
pip install -r requirements.txt

# Let's install the current app in dev mode
pip install -e .
```

- The template install by default the most recent GeoNode release from **PyPi**; we want here to link our `my_geonode` to the `geonode` currently present on the local machine

```shell
# Let's remove the currently installed GeoNode
pip uninstall GeoNode

# Let's install the local GeoNode in development mode
pip install -e /opt/geonode
pip install pygdal=="`gdal-config --version`.*"
```

We are good to go; the dependencies should be updated also. Let's prepare and start the `my_geonode` project.

```shell
# Ensure the NGINX and UWSGI services have been stopped
sudo systemctl stop nginx
sudo pkill -9 -f uwsgi
sudo pas aux | grep uwsgi

# Ensure we are in the correct virtualenv and folder
workon my_geonode
cd /opt/geonode-project/my_geonode/
```

- Let's edit the project `.env` in order to match the dev environment

### .env
```shell
cp /opt/geonode/.env_dev .
cp /opt/geonode/manage_dev.sh .
cp /opt/geonode/paver_dev.sh .
```

```shell
diff -ruN .env .env_dev
```

```diff
--- .env	2021-09-10 10:36:06.265060889 +0100
+++ .env_dev	2021-09-10 11:02:40.453757048 +0100
@@ -1,4 +1,4 @@
-COMPOSE_PROJECT_NAME=my_geonode
+COMPOSE_PROJECT_NAME=geonode
 DOCKER_HOST_IP=
 DOCKER_ENV=production
 # See https://github.com/geosolutions-it/geonode-generic/issues/28
@@ -7,71 +7,50 @@
 BACKUPS_VOLUME_DRIVER=local
 
 C_FORCE_ROOT=1
-IS_CELERY=false
 FORCE_REINIT=false
-
-SITEURL=http://localhost/
-ALLOWED_HOSTS=['django',]
+INVOKE_LOG_STDOUT=true
 
 # LANGUAGE_CODE=pt
 # LANGUAGES=(('en','English'),('pt','Portuguese'))
 
+DJANGO_SETTINGS_MODULE=geonode.settings
 GEONODE_INSTANCE_NAME=geonode
-DJANGO_SETTINGS_MODULE=my_geonode.settings
-UWSGI_CMD=uwsgi --ini /usr/src/my_geonode/uwsgi.ini
+GEONODE_LB_HOST_IP=
+GEONODE_LB_PORT=
 
 # #################
 # backend
 # #################
 POSTGRES_USER=postgres
 POSTGRES_PASSWORD=postgres
-GEONODE_DATABASE=my_geonode
+GEONODE_DATABASE=geonode
 GEONODE_DATABASE_PASSWORD=geonode
-GEONODE_GEODATABASE=my_geonode_data
+GEONODE_GEODATABASE=geonode_data
 GEONODE_GEODATABASE_PASSWORD=geonode
 GEONODE_DATABASE_SCHEMA=public
 GEONODE_GEODATABASE_SCHEMA=public
-DATABASE_HOST=db
+DATABASE_HOST=localhost
 DATABASE_PORT=5432
-DATABASE_URL=postgis://my_geonode:geonode@db:5432/my_geonode
-GEODATABASE_URL=postgis://my_geonode_data:geonode@db:5432/my_geonode_data
+DATABASE_URL=postgis://geonode:geonode@localhost:5432/geonode
+GEODATABASE_URL=postgis://geonode:geonode@localhost:5432/geonode_data
 GEONODE_DB_CONN_MAX_AGE=0
 GEONODE_DB_CONN_TOUT=5
 DEFAULT_BACKEND_DATASTORE=datastore
-BROKER_URL=amqp://guest:guest@rabbitmq:5672/
-ASYNC_SIGNALS=True
+BROKER_URL=amqp://admin:admin@localhost:5672//
+ASYNC_SIGNALS=False
 
-# #################
-# geoserver
-# #################
-GEOSERVER_WEB_UI_LOCATION=http://localhost/geoserver/
-GEOSERVER_PUBLIC_LOCATION=http://localhost/geoserver/
-GEOSERVER_LOCATION=http://geoserver:8080/geoserver/
-GEOSERVER_ADMIN_USER=admin
-GEOSERVER_ADMIN_PASSWORD=geoserver
-
-OGC_REQUEST_TIMEOUT=30
-OGC_REQUEST_MAX_RETRIES=1
-OGC_REQUEST_BACKOFF_FACTOR=0.3
-OGC_REQUEST_POOL_MAXSIZE=10
-OGC_REQUEST_POOL_CONNECTIONS=10
+SITEURL=http://localhost:8000/
 
-# Java Options & Memory
-ENABLE_JSONP=true
-outFormat=text/javascript
-GEOSERVER_JAVA_OPTS=-Djava.awt.headless=true -Xms2G -Xmx4G -XX:+UnlockDiagnosticVMOptions -XX:+LogVMOutput -XX:LogFile=/var/log/jvm.log -XX:PerfDataSamplingInterval=500 -XX:SoftRefLRUPolicyMSPerMB=36000 -XX:-UseGCOverheadLimit -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:ParallelGCThreads=4 -Dfile.encoding=UTF8 -Djavax.servlet.request.encoding=UTF-8 -Djavax.servlet.response.encoding=UTF-8 -Duser.timezone=GMT -Dorg.geotools.shapefile.datetime=false -DGEOSERVER_CSRF_DISABLED=true -DPRINT_BASE_URL=http://geoserver:8080/geoserver/pdf -DALLOW_ENV_PARAMETRIZATION=true -Xbootclasspath/a:/usr/local/tomcat/webapps/geoserver/WEB-INF/lib/marlin-0.9.3-Unsafe.jar -Dsun.java2d.renderer=org.marlin.pisces.MarlinRenderingEngine
+ALLOWED_HOSTS="['django', '*']"
 
 # Data Uploader
 DEFAULT_BACKEND_UPLOADER=geonode.importer
 TIME_ENABLED=True
 MOSAIC_ENABLED=False
-
-# #################
-# Jenkins
-# CI/CD Server
-# #################
-JENKINS_HTTP_PORT=9080
-JENKINS_HTTPS_PORT=9443
+HAYSTACK_SEARCH=False
+HAYSTACK_ENGINE_URL=http://elasticsearch:9200/
+HAYSTACK_ENGINE_INDEX_NAME=haystack
+HAYSTACK_SEARCH_RESULTS_PER_PAGE=200
 
 # #################
 # nginx
@@ -85,7 +64,7 @@
 HTTP_HOST=localhost
 HTTPS_HOST=
 
-HTTP_PORT=80
+HTTP_PORT=8000
 HTTPS_PORT=443
 
 # Let's Encrypt certificates for https encryption. You must have a domain name as HTTPS_HOST (doesn't work
@@ -100,15 +79,29 @@
 RESOLVER=127.0.0.11
 
 # #################
+# geoserver
+# #################
+GEOSERVER_WEB_UI_LOCATION=http://localhost:8080/geoserver/
+GEOSERVER_PUBLIC_LOCATION=http://localhost:8080/geoserver/
+GEOSERVER_LOCATION=http://localhost:8080/geoserver/
+GEOSERVER_ADMIN_USER=admin
+GEOSERVER_ADMIN_PASSWORD=geoserver
+
+OGC_REQUEST_TIMEOUT=5
+OGC_REQUEST_MAX_RETRIES=1
+OGC_REQUEST_BACKOFF_FACTOR=0.3
+OGC_REQUEST_POOL_MAXSIZE=10
+OGC_REQUEST_POOL_CONNECTIONS=10
+
+# Java Options & Memory
+ENABLE_JSONP=true
+outFormat=text/javascript
+GEOSERVER_JAVA_OPTS="-Djava.awt.headless=true -Xms2G -Xmx4G -XX:+UnlockDiagnosticVMOptions -XX:+LogVMOutput -XX:LogFile=/var/log/jvm.log -XX:PerfDataSamplingInterval=500 -XX:SoftRefLRUPolicyMSPerMB=36000 -XX:-UseGCOverheadLimit -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:ParallelGCThreads=4 -Dfile.encoding=UTF8 -Djavax.servlet.request.encoding=UTF-8 -Djavax.servlet.response.encoding=UTF-8 -Duser.timezone=GMT -Dorg.geotools.shapefile.datetime=false -DGEOSERVER_CSRF_DISABLED=true -DPRINT_BASE_URL=http://geoserver:8080/geoserver/pdf -DALLOW_ENV_PARAMETRIZATION=true -Xbootclasspath/a:/usr/local/tomcat/webapps/geoserver/WEB-INF/lib/marlin-0.9.3-Unsafe.jar -Dsun.java2d.renderer=org.marlin.pisces.MarlinRenderingEngine"
+
+# #################
 # Security
 # #################
 # Admin Settings
-#
-# ADMIN_PASSWORD is used to overwrite the GeoNode admin password **ONLY** the first time
-# GeoNode is run. If you need to overwrite it again, you need to set the env var FORCE_REINIT,
-# otherwise the invoke updateadmin task will be skipped and the current password already stored
-# in DB will honored.
-
 ADMIN_USERNAME=admin
 ADMIN_PASSWORD=admin
 ADMIN_EMAIL=admin@localhost
@@ -127,7 +120,7 @@
 # Session/Access Control
 LOCKDOWN_GEONODE=False
 CORS_ORIGIN_ALLOW_ALL=True
-X_FRAME_OPTIONS=SAMEORIGIN
+X_FRAME_OPTIONS="SAMEORIGIN"
 SESSION_EXPIRED_CONTROL_ENABLED=True
 DEFAULT_ANONYMOUS_VIEW_PERMISSION=True
 DEFAULT_ANONYMOUS_DOWNLOAD_PERMISSION=True
@@ -156,13 +149,13 @@
 # Production and
 # Monitoring
 # #################
-DEBUG=False
+DEBUG=True
 
 SECRET_KEY='myv-y4#7j-d*p-__@j#*3z@!y24fz8%^z2v6atuy4bo9vqr1_a'
 
-STATIC_ROOT=/mnt/volumes/statics/static/
-MEDIA_ROOT=/mnt/volumes/statics/uploaded/
-GEOIP_PATH=/mnt/volumes/statics/geoip.db
+# STATIC_ROOT=/mnt/volumes/statics/static/
+# MEDIA_ROOT=/mnt/volumes/statics/uploaded/
+# GEOIP_PATH=/mnt/volumes/statics/geoip.db
 
 CACHE_BUSTING_STATIC_ENABLED=False
 CACHE_BUSTING_MEDIA_ENABLED=False
@@ -198,32 +191,3 @@
 EXIF_ENABLED=True
 CREATE_LAYER=True
 FAVORITE_ENABLED=True
-
-# LDAP
-LDAP_ENABLED=False
-LDAP_SERVER_URL=ldap://<the_ldap_server>
-LDAP_BIND_DN=uid=ldapinfo,cn=users,dc=ad,dc=example,dc=org
-LDAP_BIND_PASSWORD=<something_secret>
-LDAP_USER_SEARCH_DN=dc=ad,dc=example,dc=org
-LDAP_USER_SEARCH_FILTERSTR=(&(uid=%(user)s)(objectClass=person))
-LDAP_GROUP_SEARCH_DN=cn=groups,dc=ad,dc=example,dc=org
-LDAP_GROUP_SEARCH_FILTERSTR=(|(cn=abt1)(cn=abt2)(cn=abt3)(cn=abt4)(cn=abt5)(cn=abt6))
-LDAP_GROUP_PROFILE_MEMBER_ATTR=uniqueMember
-
-# CELERY
-
-# expressed in KB
-# CELERY__MAX_MEMORY_PER_CHILD="200000"
-# ## 
-# Note right autoscale value must coincide with worker concurrency value
-# CELERY__AUTOSCALE_VALUES="1,4" 
-# CELERY__WORKER_CONCURRENCY="4"
-# ##
-# CELERY__OPTS="--without-gossip --without-mingle -Ofair -B -E"
-# CELERY__BEAT_SCHEDULE="/mnt/volumes/statics/celerybeat-schedule"
-# CELERY__LOG_LEVEL="INFO"
-# CELERY__LOG_FILE="/var/log/celery.log"
-# CELERY__WORKER_NAME="worker1@%h"
-
-# PostgreSQL
-POSTGRESQL_MAX_CONNECTIONS=200
```

**IMPORTANT**: _Make sure you are using the correct_ `settings` _file_

```shell
vim .env_dev
```

```ini
DJANGO_SETTINGS_MODULE=my_geonode.settings
GEONODE_INSTANCE_NAME=my_geonode
```

### Make sure the DB and GeoNode are aligned

```shell
./paver_dev.sh sync
```

**WARNING**: _Whenever you face some issues with the migrations, please try upgrading the dependency as shown below_

```shell
pip install geonode-oauth-toolkit==2.2.2
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
### Start `my_geonode` in DEV Mode
```shell
./paver_dev.sh start_django
```

- **Notice** that we don't need to fix GeoServer `OAuth2` endpoints since we did already on the previous section
- Open the browser and go to the location `http://localhost:8000`
- The main page and theme have been changed again

![image](https://user-images.githubusercontent.com/1278021/132838719-068a6cd4-65df-43b7-81b0-853293b628b3.png)

#### [Next Section: Programmatically Customize the geonode-project instance](GEONODE_PROJ_CUST.md)
