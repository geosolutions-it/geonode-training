# Put geonode-project in Production
In this section we will update `NGINX` and `UWSGI` daemons in order to point to our brand new `geonode-project` instance

- Stop any current running instance of GeoNode

```shell
workon my_geonode
cd /opt/geonode-project/my_geonode

./manage_dev.sh stop_django

sudo systemctl stop nginx
sudo pkill -9 uwsgi
```

- Create the new `UWSGI app`

```shell
sudo cp /etc/uwsgi/apps-available/geonode.ini /etc/uwsgi/apps-available/my_geonode.ini
sudo rm /etc/uwsgi/apps-enabled/geonode.ini
sudo ln -s /etc/uwsgi/apps-available/my_geonode.ini /etc/uwsgi/apps-enabled/my_geonode.ini
```

```shell
sudo vim /etc/uwsgi/apps-enabled/my_geonode.ini
```
```diff
--- /etc/uwsgi/apps-available/geonode.ini	2021-07-15 13:59:22.686191424 +0100
+++ /etc/uwsgi/apps-available/my_geonode.ini	2021-09-14 15:21:57.216587290 +0100
@@ -5,12 +5,12 @@
 gid = www-data
 
 plugins = python3
-virtualenv = /home/geonode-vm-321/.virtualenvs/geonode
+virtualenv = /home/geonode-vm-321/.virtualenvs/my_geonode
 
 env = DEBUG=False
 
-env = DJANGO_SETTINGS_MODULE=geonode.settings
-env = GEONODE_INSTANCE_NAME=geonode
+env = DJANGO_SETTINGS_MODULE=my_geonode.settings
+env = GEONODE_INSTANCE_NAME=my_geonode
 env = GEONODE_LB_HOST_IP=
 env = GEONODE_LB_PORT=
```

- Restart the services

```shell
sudo systemctl start nginx
sudo /etc/init.d/uwsgi restart
```

    ![image](https://user-images.githubusercontent.com/1278021/133278507-67af435b-f35b-4001-8f21-2ea5f0b840de.png)

- Fix `links` and `paths`

```shell
cp .env_dev .env_prod
vim .env_prod
```
```diff
--- .env_dev	2021-09-10 11:29:22.415720387 +0100
+++ .env_prod	2021-09-14 15:32:01.768587290 +0100
@@ -39,7 +39,7 @@
 BROKER_URL=amqp://admin:admin@localhost:5672//
 ASYNC_SIGNALS=False
 
-SITEURL=http://localhost:8000/
+SITEURL=http://localhost/
 
 ALLOWED_HOSTS="['django', '*']"
 
@@ -81,9 +81,9 @@
 # #################
 # geoserver
 # #################
-GEOSERVER_WEB_UI_LOCATION=http://localhost:8080/geoserver/
-GEOSERVER_PUBLIC_LOCATION=http://localhost:8080/geoserver/
-GEOSERVER_LOCATION=http://localhost:8080/geoserver/
+GEOSERVER_WEB_UI_LOCATION=http://localhost/geoserver/
+GEOSERVER_PUBLIC_LOCATION=http://localhost/geoserver/
+GEOSERVER_LOCATION=http://localhost/geoserver/
 GEOSERVER_ADMIN_USER=admin
 GEOSERVER_ADMIN_PASSWORD=geoserver
 
@@ -96,7 +96,7 @@
 # Java Options & Memory
 ENABLE_JSONP=true
 outFormat=text/javascript
-GEOSERVER_JAVA_OPTS="-Djava.awt.headless=true -Xms2G -Xmx4G -XX:+UnlockDiagnosticVMOptions -XX:+LogVMOutput -XX:LogFile=/var/log/jvm.log -XX:PerfDataSamplingInterval=500 -XX:SoftRefLRUPolicyMSPerMB=36000 -XX:-UseGCOverheadLimit -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:ParallelGCThreads=4 -Dfile.encoding=UTF8 -Djavax.servlet.request.encoding=UTF-8 -Djavax.servlet.response.encoding=UTF-8 -Duser.timezone=GMT -Dorg.geotools.shapefile.datetime=false -DGEOSERVER_CSRF_DISABLED=true -DPRINT_BASE_URL=http://geoserver:8080/geoserver/pdf -DALLOW_ENV_PARAMETRIZATION=true -Xbootclasspath/a:/usr/local/tomcat/webapps/geoserver/WEB-INF/lib/marlin-0.9.3-Unsafe.jar -Dsun.java2d.renderer=org.marlin.pisces.MarlinRenderingEngine"
+GEOSERVER_JAVA_OPTS="-Djava.awt.headless=true -Xms2G -Xmx4G -XX:+UnlockDiagnosticVMOptions -XX:+LogVMOutput -XX:LogFile=/var/log/jvm.log -XX:PerfDataSamplingInterval=500 -XX:SoftRefLRUPolicyMSPerMB=36000 -XX:-UseGCOverheadLimit -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:ParallelGCThreads=4 -Dfile.encoding=UTF8 -Djavax.servlet.request.encoding=UTF-8 -Djavax.servlet.response.encoding=UTF-8 -Duser.timezone=GMT -Dorg.geotools.shapefile.datetime=false -DGEOSERVER_CSRF_DISABLED=true -DPRINT_BASE_URL=http://localhost/geoserver/pdf -DALLOW_ENV_PARAMETRIZATION=true -Xbootclasspath/a:/usr/local/tomcat/webapps/geoserver/WEB-INF/lib/marlin-0.9.3-Unsafe.jar -Dsun.java2d.renderer=org.marlin.pisces.MarlinRenderingEngine"
 
 # #################
 # Security
```

```shell
cp manage_dev.sh manage_prod.sh
vim manage_prod.sh 
```
```diff
--- manage_dev.sh	2021-09-10 10:42:26.571118944 +0100
+++ manage_prod.sh	2021-09-14 15:30:36.132587290 +0100
@@ -1,5 +1,5 @@
 set -a
-. ./.env_dev
+. ./.env_prod
 set +a
 
 python manage.py $@
```

```shell
cp paver_dev.sh paver_prod.sh
vim paver_prod.sh 
```
```diff
--- paver_dev.sh	2021-09-10 10:42:35.615638950 +0100
+++ paver_prod.sh	2021-09-14 15:30:48.284587290 +0100
@@ -1,5 +1,5 @@
 set -a
-. ./.env_dev
+. ./.env_prod
 set +a
 
 paver $@
```

```shell
./manage_prod.sh collectstatic
./manage_prod.sh migrate_baseurl --source-address=localhost:8000 --target-address=localhost
./manage_prod.sh migrate_baseurl --source-address=localhost:8080 --target-address=localhost
```

- Update GeoServer `http://localhost/geoserver/`

    * Login as `admin`
    ![image](https://user-images.githubusercontent.com/1278021/133279589-24fcad74-3d9e-414d-a08a-78fb3cc4ca29.png)

    * Update the global `PROXY BASE URL`
    ![image](https://user-images.githubusercontent.com/1278021/133279756-dc21be9a-2ec8-43ae-8717-7511845cd8ae.png)

    * Update the `Role Service`
    ![image](https://user-images.githubusercontent.com/1278021/133279927-d1e915ad-af37-4ded-b368-fad9adb763a8.png)

    ![image](https://user-images.githubusercontent.com/1278021/133280203-5c035ef0-d9df-4044-a8c2-123ed49e8cb2.png)

    * Update the `OAUTH2` security service
    ![image](https://user-images.githubusercontent.com/1278021/133280407-4fe6fdfa-60c3-41f0-b329-d05f2f28efe5.png)

    ![image](https://user-images.githubusercontent.com/1278021/133280534-1f5db18b-f528-441b-ab1e-618466ca60d3.png)

- Synchronize Layer Metadata

```shell
./manage_prod.sh set_all_layers_metadata -d
```

```shell
./manage_dev.sh sync_geonode_layers --updatepermissions --updateattributes
```

#### [Next Section: Upgrade GeoNode to Latest Version](100_GEONODE_UPGRADE.md)
