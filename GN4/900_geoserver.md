# Setup GeoServer

Let’s install Tomcat and edit the default startup script.
```
sudo apt install tomcat9 -y
sudo systemctl edit tomcat9
```

Edit the startup script with the following content:

```
[Unit]
Description=Apache Tomcat Server
After=syslog.target network.target
RequiresMountsFor=/var/log/geoserver /opt/data/geoserver_data /opt/data/geoserver_cache

[Service]

# Configuration
Environment="JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/"
Environment="GEOSERVER_DATA_DIR=/opt/data/geoserver_data"
Environment="GEOSERVER_LOG_LOCATION=/var/log/geoserver/geoserver.log"
Environment="GEOWEBCACHE_CACHE_DIR=/opt/data/geoserver_cache"
Environment="GEOFENCE_DIR=${GEOSERVER_DATA_DIR}/geofence"
Environment="TIMEZONE=UTC"
Environment="JAVA_OPTS='-server \
    -Djava.awt.headless=true \
    -Dorg.geotools.shapefile.datetime=false \
    -XX:+UseParallelGC \
    -XX:ParallelGCThreads=4 \
    -Dfile.encoding=UTF8 \
    -Duser.timezone=${TIMEZONE} \
    -Xms1024m \
    -Xmx2048m \
    -Djavax.servlet.request.encoding=UTF-8 \
    -Djavax.servlet.response.encoding=UTF-8 \
    -DGEOSERVER_CSRF_DISABLED=true \
    -DPRINT_BASE_URL=http://localhost:8080/geoserver/pdf \
    -DGEOSERVER_DATA_DIR=${GEOSERVER_DATA_DIR} \
    -DGEOSERVER_LOG_LOCATION=${GEOSERVER_LOG_LOCATION} \
    -Dgeofence.dir=${GEOFENCE_DIR} \
    -DGEOWEBCACHE_CACHE_DIR=${GEOWEBCACHE_CACHE_DIR}'"

# Security
ReadWritePaths=/opt/data/geoserver_data
ReadWritePaths=/var/log/geoserver
ReadWritePaths=/opt/data/geoserver_cache

[Install]
WantedBy=multi-user.target
```

Create the Directory Structure and Install Needed Packages

Stop running tomcat for geoserver provisioning
```
systemctl stop tomcat9
```

Create the geoserver directories
```
sudo mkdir -p /opt/data/geoserver_data
sudo mkdir -p /var/log/geoserver
sudo mkdir -p /opt/data/geoserver_cache
```
Set the directory permissions
```
sudo chmod 0755 /opt/data/geoserver_data
sudo chmod 2750 /var/log/geoserver
sudo chmod 0750 /opt/data/geoserver_cache
```

Install the data directory template
```
cd /opt/data/geoserver_data
sudo wget --no-check-certificate "https://artifacts.geonode.org/geoserver/2.18.x/geonode-geoserver-ext-web-app-data.zip" -O data-2.18.3.zip
sudo unzip data-2.18.3.zip
```

Set geoserver directory ownership
```
sudo chown -R tomcat:tomcat /opt/data/geoserver_data
sudo chown -R tomcat:adm /var/log/geoserver
sudo chown -R tomcat:tomcat /opt/data/geoserver_cache
```

Temporary placeholder for download war file
```
cd /opt && sudo mkdir -p geoserver && cd geoserver
```

Install geoserver war
```
sudo wget --no-check-certificate "https://artifacts.geonode.org/geoserver/2.18.x/geoserver.war" -O geoserver.war
sudo mv geoserver.war /var/lib/tomcat9/webapps
```

Start Tomcat9
```
sudo systemctl start tomcat9
```

Check if GeoServer started by pointing the browser to http://localhost:8080/geoserver

## Troubleshooting

If tomcat is not starting GeoServer due to an error containing this snipped of stacktrace
```
Caused by: java.lang.IllegalStateException: Unable to complete the scan for annotations for web application [/geoserver] due to a StackOverflowError. 
Possible root causes include a too low setting for -Xss and illegal cyclic inheritance dependencies. 
The class hierarchy being processed was [org.bouncycastle.asn1.ASN1EncodableVector>org.bouncycastle.asn1.DEREncodableVector->org.bouncycastle.asn1.ASN1EncodableVector]
```

you may need to edit the `catalina.properties` file:
```
sudo sed -i -e 's/xom-\*\.jar/xom-\*\.jar,bcprov\*\.jar/g' /var/lib/tomcat9/conf/catalina.properties
```
and restart tomcat.
