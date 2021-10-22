.. _geoserver:

=================
GeoServer Install
=================

In this section, we are going to install the ``Apache Tomcat 9`` Servlet Java container, which will be started by default on the internal port ``8080``.

We will also perform several optimizations to:

1. Correctly setup the Java VM Options, like the available heap memory and the garbage collector options.
2. Externalize the ``GeoServer`` and ``GeoWebcache`` catalogs in order to allow further updates without the risk of deleting our datasets.

.. note:: This is still a basic setup of those components. More details will be provided on sections of the documentation concerning the hardening of the system in a production environment. Nevertheless, you will need to tweak a bit those settings accordingly with your current system. As an instance, if your machine does not have enough memory, you will need to lower down the initial amount of available heap memory. **Warnings** and **notes** will be placed below the statements that will require your attention.

**Install Apache Tomcat 9 (ref. https://yallalabs.com/linux/ubuntu/how-to-install-apache-tomcat-9-ubuntu-20-04/)**

.. warning:: Apache Tomcat 9 requires Java 8 or newer to be installed on the server.
  Check the steps before in order to be sure you have OpenJDK 8 correctly installed on your system.

Let's install Tomcat and edit the default startup script.

.. code-block:: shell

  sudo apt install tomcat9 -y
  sudo systemctl edit tomcat9

Edit the startup script with the following content:

.. code-block:: shell

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

Create the Directory Structure and Install Needed Packages

.. code-block:: shell

  ## stop running tomcat for geoserver provisioning
  systemctl stop tomcat9

  ## set geoserver version
  export GS_VERSION=2.18.3

  ## make the geoserver directories
  sudo mkdir -p /opt/data/geoserver_data
  sudo mkdir -p /var/log/geoserver
  sudo mkdir -p /opt/data/geoserver_cache

  ## set the directory permissions
  sudo chmod 0755 /opt/data/geoserver_data
  sudo chmod 2750 /var/log/geoserver
  sudo chmod 0750 /opt/data/geoserver_cache

  ## install the data directory template
  cd /opt/data/geoserver_data
  sudo wget --no-check-certificate "https://artifacts.geonode.org/geoserver/2.18.x/geonode-geoserver-ext-web-app-data.zip" -O data-$GS_VERSION.zip
  sudo unzip data-$GS_VERSION.zip

  ## set geoserver directory ownership
  sudo chown -R tomcat:tomcat /opt/data/geoserver_data
  sudo chown -R tomcat:adm /var/log/geoserver
  sudo chown -R tomcat:tomcat /opt/data/geoserver_cache

  ## temporary placeholder for download war file
  cd /opt && sudo mkdir -p geoserver && cd geoserver

  ## install geoserver war
  sudo wget --no-check-certificate "https://artifacts.geonode.org/geoserver/2.18.x/geoserver.war" -O geoserver-$GS_VERSION.war
  sudo mv geoserver-$GS_VERSION.war /var/lib/tomcat9/webapps/geoserver.war

  ## start Tomcat9
  sudo systemctl start tomcat9
