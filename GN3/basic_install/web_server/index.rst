.. _web_server:

==========
Web Server
==========

In this section we will see:

1. How to configure ``NGINX`` HTTPD Server to host ``GeoNode`` and ``GeoServer``. In the initial setup we will still run the services on ``http://localhost``
2. Update the ``settings`` in order to link ``GeoNode`` and ``GeoServer`` to the ``PostgreSQL`` Database.

Install and configure NGINX and UWSGI
.....................................

.. code-block:: shell

  # Install the services
  sudo apt install -y nginx uwsgi uwsgi-plugin-python3
  sudo mkdir -p /var/log/geonode/

Serving {“geonode”, “geoserver”} via NGINX
..........................................

.. code-block:: shell

  # Create the GeoNode UWSGI config
  sudo vim /etc/uwsgi/apps-available/geonode.ini

.. warning:: **!IMPORTANT!**

    Change the line ``virtualenv = /home/<my_user>/.virtualenvs/geonode`` below with your current user home directory!

    e.g.: If the user is ``afabiani`` then ``virtualenv = /home/afabiani/.virtualenvs/geonode``

.. code-block:: ini

  [uwsgi]
  uwsgi-socket = 0.0.0.0:8000
  # http-socket = 0.0.0.0:8000

  gid = www-data

  plugins = python3
  virtualenv = /home/<my_user>/.virtualenvs/geonode

  env = DJANGO_SETTINGS_MODULE=geonode.settings
  env = GEONODE_INSTANCE_NAME=geonode
  env = GEONODE_LB_HOST_IP=
  env = GEONODE_LB_PORT=

  # #################
  # backend
  # #################
  env = POSTGRES_USER=postgres
  env = POSTGRES_PASSWORD=postgres
  env = GEONODE_DATABASE=geonode
  env = GEONODE_DATABASE_PASSWORD=geonode
  env = GEONODE_GEODATABASE=geonode_data
  env = GEONODE_GEODATABASE_PASSWORD=geonode
  env = GEONODE_DATABASE_SCHEMA=public
  env = GEONODE_GEODATABASE_SCHEMA=public
  env = DATABASE_HOST=localhost
  env = DATABASE_PORT=5432
  env = DATABASE_URL=postgis://geonode:geonode@localhost:5432/geonode
  env = GEODATABASE_URL=postgis://geonode:geonode@localhost:5432/geonode_data
  env = GEONODE_DB_CONN_MAX_AGE=0
  env = GEONODE_DB_CONN_TOUT=5
  env = DEFAULT_BACKEND_DATASTORE=datastore
  env = BROKER_URL=amqp://admin:admin@localhost:5672//
  env = ASYNC_SIGNALS=False

  env = SITEURL=http://localhost/

  env = ALLOWED_HOSTS="['*']"

  # Data Uploader
  env = DEFAULT_BACKEND_UPLOADER=geonode.importer
  env = TIME_ENABLED=True
  env = MOSAIC_ENABLED=False
  env = HAYSTACK_SEARCH=False
  env = HAYSTACK_ENGINE_URL=http://elasticsearch:9200/
  env = HAYSTACK_ENGINE_INDEX_NAME=haystack
  env = HAYSTACK_SEARCH_RESULTS_PER_PAGE=200

  # #################
  # nginx
  # HTTPD Server
  # #################
  env = GEONODE_LB_HOST_IP=localhost
  env = GEONODE_LB_PORT=80

  # IP or domain name and port where the server can be reached on HTTPS (leave HOST empty if you want to use HTTP only)
  # port where the server can be reached on HTTPS
  env = HTTP_HOST=localhost
  env = HTTPS_HOST=

  env = HTTP_PORT=8000
  env = HTTPS_PORT=443

  # #################
  # geoserver
  # #################
  env = GEOSERVER_WEB_UI_LOCATION=http://localhost/geoserver/
  env = GEOSERVER_PUBLIC_LOCATION=http://localhost/geoserver/
  env = GEOSERVER_LOCATION=http://localhost:8080/geoserver/
  env = GEOSERVER_ADMIN_USER=admin
  env = GEOSERVER_ADMIN_PASSWORD=geoserver

  env = OGC_REQUEST_TIMEOUT=5
  env = OGC_REQUEST_MAX_RETRIES=1
  env = OGC_REQUEST_BACKOFF_FACTOR=0.3
  env = OGC_REQUEST_POOL_MAXSIZE=10
  env = OGC_REQUEST_POOL_CONNECTIONS=10

  # Java Options & Memory
  env = ENABLE_JSONP=true
  env = outFormat=text/javascript
  env = GEOSERVER_JAVA_OPTS="-Djava.awt.headless=true -Xms1G -Xmx2G -XX:+UnlockDiagnosticVMOptions -XX:+LogVMOutput -XX:LogFile=/var/log/jvm.log -XX:PerfDataSamplingInterval=500 -XX:SoftRefLRUPolicyMSPerMB=36000 -XX:-UseGCOverheadLimit -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:ParallelGCThreads=4 -Dfile.encoding=UTF8 -Djavax.servlet.request.encoding=UTF-8 -Djavax.servlet.response.encoding=UTF-8 -Duser.timezone=GMT -Dorg.geotools.shapefile.datetime=false -DGEOSERVER_CSRF_DISABLED=true -DPRINT_BASE_URL=http://geoserver:8080/geoserver/pdf -DALLOW_ENV_PARAMETRIZATION=true -Xbootclasspath/a:/usr/local/tomcat/webapps/geoserver/WEB-INF/lib/marlin-0.9.3-Unsafe.jar -Dsun.java2d.renderer=org.marlin.pisces.MarlinRenderingEngine"

  # #################
  # Security
  # #################
  # Admin Settings
  env = ADMIN_USERNAME=admin
  env = ADMIN_PASSWORD=admin
  env = ADMIN_EMAIL=admin@localhost

  # EMAIL Notifications
  env = EMAIL_ENABLE=False
  env = DJANGO_EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
  env = DJANGO_EMAIL_HOST=localhost
  env = DJANGO_EMAIL_PORT=25
  env = DJANGO_EMAIL_HOST_USER=
  env = DJANGO_EMAIL_HOST_PASSWORD=
  env = DJANGO_EMAIL_USE_TLS=False
  env = DJANGO_EMAIL_USE_SSL=False
  env = DEFAULT_FROM_EMAIL='GeoNode <no-reply@geonode.org>'

  # Session/Access Control
  env = LOCKDOWN_GEONODE=False
  env = CORS_ORIGIN_ALLOW_ALL=True
  env = X_FRAME_OPTIONS="SAMEORIGIN"
  env = SESSION_EXPIRED_CONTROL_ENABLED=True
  env = DEFAULT_ANONYMOUS_VIEW_PERMISSION=True
  env = DEFAULT_ANONYMOUS_DOWNLOAD_PERMISSION=True

  # Users Registration
  env = ACCOUNT_OPEN_SIGNUP=True
  env = ACCOUNT_EMAIL_REQUIRED=True
  env = ACCOUNT_APPROVAL_REQUIRED=False
  env = ACCOUNT_CONFIRM_EMAIL_ON_GET=False
  env = ACCOUNT_EMAIL_VERIFICATION=none
  env = ACCOUNT_EMAIL_CONFIRMATION_EMAIL=False
  env = ACCOUNT_EMAIL_CONFIRMATION_REQUIRED=False
  env = ACCOUNT_AUTHENTICATION_METHOD=username_email
  env = AUTO_ASSIGN_REGISTERED_MEMBERS_TO_REGISTERED_MEMBERS_GROUP_NAME=True

  # OAuth2
  env = OAUTH2_API_KEY=
  env = OAUTH2_CLIENT_ID=Jrchz2oPY3akmzndmgUTYrs9gczlgoV20YPSvqaV
  env = OAUTH2_CLIENT_SECRET=rCnp5txobUo83EpQEblM8fVj3QT5zb5qRfxNsuPzCqZaiRyIoxM4jdgMiZKFfePBHYXCLd7B8NlkfDBY9HKeIQPcy5Cp08KQNpRHQbjpLItDHv12GvkSeXp6OxaUETv3

  # GeoNode APIs
  env = API_LOCKDOWN=False
  env = TASTYPIE_APIKEY=

  # #################
  # Production and
  # Monitoring
  # #################
  env = DEBUG=False

  SECRET_KEY='myv-y4#7j-d*p-__@j#*3z@!y24fz8%^z2v6atuy4bo9vqr1_a'

  env = CACHE_BUSTING_STATIC_ENABLED=False
  env = CACHE_BUSTING_MEDIA_ENABLED=False

  env = MEMCACHED_ENABLED=False
  env = MEMCACHED_BACKEND=django.core.cache.backends.memcached.MemcachedCache
  env = MEMCACHED_LOCATION=127.0.0.1:11211
  env = MEMCACHED_LOCK_EXPIRE=3600
  env = MEMCACHED_LOCK_TIMEOUT=10

  env = MAX_DOCUMENT_SIZE=2
  env = CLIENT_RESULTS_LIMIT=5
  env = API_LIMIT_PER_PAGE=1000

  # GIS Client
  env = GEONODE_CLIENT_LAYER_PREVIEW_LIBRARY=mapstore
  env = MAPBOX_ACCESS_TOKEN=
  env = BING_API_KEY=
  env = GOOGLE_API_KEY=

  # Monitoring
  env = MONITORING_ENABLED=False
  env = MONITORING_DATA_TTL=365
  env = USER_ANALYTICS_ENABLED=True
  env = USER_ANALYTICS_GZIP=True
  env = CENTRALIZED_DASHBOARD_ENABLED=False
  env = MONITORING_SERVICE_NAME=local-geonode
  env = MONITORING_HOST_NAME=geonode

  # Other Options/Contribs
  env = MODIFY_TOPICCATEGORY=True
  env = AVATAR_GRAVATAR_SSL=True
  env = EXIF_ENABLED=True
  env = CREATE_LAYER=True
  env = FAVORITE_ENABLED=True

  # pidfile = /tmp/geonode.pid

  chdir = /opt/geonode
  module = geonode.wsgi:application

  strict = false
  master = true
  enable-threads = true
  vacuum = true                        ; Delete sockets during shutdown
  single-interpreter = true
  die-on-term = true                   ; Shutdown when receiving SIGTERM (default is respawn)
  need-app = true

  # logging
  # path to where uwsgi logs will be saved
  # logto = /opt/data/geonode_logs/geonode.log

  logto = /var/log/geonode/geonode.log
  daemonize = /var/log/geonode/geonode.log
  touch-reload = /opt/geonode/geonode/wsgi.py
  buffer-size = 32768

  harakiri = 60                        ; forcefully kill workers after 60 seconds
  py-callos-afterfork = true           ; allow workers to trap signals

  max-requests = 1000                  ; Restart workers after this many requests
  max-worker-lifetime = 3600           ; Restart workers after this many seconds
  reload-on-rss = 2048                 ; Restart workers after this much resident memory
  worker-reload-mercy = 60             ; How long to wait before forcefully killing workers

  cheaper-algo = busyness
  processes = 128                      ; Maximum number of workers allowed
  cheaper = 8                          ; Minimum number of workers allowed
  cheaper-initial = 16                 ; Workers created at startup
  cheaper-overload = 1                 ; Length of a cycle in seconds
  cheaper-step = 16                    ; How many workers to spawn at a time

  cheaper-busyness-multiplier = 30     ; How many cycles to wait before killing workers
  cheaper-busyness-min = 20            ; Below this threshold, kill workers (if stable for multiplier cycles)
  cheaper-busyness-max = 70            ; Above this threshold, spawn new workers
  cheaper-busyness-backlog-alert = 16  ; Spawn emergency workers if more than this many requests are waiting in the queue
  cheaper-busyness-backlog-step = 2    ; How many emergency workers to create if there are too many requests in the queue

.. code-block:: shell

  # Enable the GeoNode UWSGI config
  sudo ln -s /etc/uwsgi/apps-available/geonode.ini /etc/uwsgi/apps-enabled/geonode.ini

  # Restart UWSGI Service
  sudo pkill -9 -f uwsgi
  sudo service uwsgi restart

.. code-block:: shell

  # Backup the original NGINX config
  sudo mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.orig

  # Create the GeoNode Default NGINX config
  sudo vim /etc/nginx/nginx.conf

.. code-block:: shell

  # Make sure your nginx.config matches the following one
  user www-data;
  worker_processes auto;
  pid /run/nginx.pid;
  include /etc/nginx/modules-enabled/*.conf;

  events {
    worker_connections 768;
    # multi_accept on;
  }

  http {
    ##
    # Basic Settings
    ##

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    # server_tokens off;

    # server_names_hash_bucket_size 64;
    # server_name_in_redirect off;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    ##
    # SSL Settings
    ##

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
    ssl_prefer_server_ciphers on;

    ##
    # Logging Settings
    ##

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    ##
    # Gzip Settings
    ##

    gzip on;
    gzip_vary on;
    gzip_proxied any;
    gzip_http_version 1.1;
    gzip_disable "MSIE [1-6]\.";
    gzip_buffers 16 8k;
    gzip_min_length 1100;
    gzip_comp_level 6;
    gzip_types video/mp4 text/plain application/javascript application/x-javascript text/javascript text/xml text/css image/jpeg;

    ##
    # Virtual Host Configs
    ##

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
  }

.. code-block:: shell

  # Remove the Default NGINX config
  sudo rm /etc/nginx/sites-enabled/default

  # Create the GeoNode App NGINX config
  sudo vim /etc/nginx/sites-available/geonode

.. code-block:: shell

  uwsgi_intercept_errors on;

  upstream geoserver_proxy {
    server localhost:8080;
  }

  # Expires map
  map $sent_http_content_type $expires {
    default                    off;
    text/html                  epoch;
    text/css                   max;
    application/javascript     max;
    ~image/                    max;
  }

  server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    server_name _;

    charset utf-8;

    etag on;
    expires $expires;
    proxy_read_timeout 600s;
    # set client body size to 2M #
    client_max_body_size 50000M;

    location / {
      etag off;
      uwsgi_pass 127.0.0.1:8000;
      uwsgi_read_timeout 600s;
      include uwsgi_params;
    }

    location /static/ {
      alias /opt/geonode/geonode/static_root/;
    }

    location /uploaded/ {
      alias /opt/geonode/geonode/uploaded/;
    }

    location /geoserver {
      proxy_pass http://geoserver_proxy;
      include proxy_params;
    }
  }

.. code-block:: shell

  # Prepare the uploaded folder
  sudo mkdir -p /opt/geonode/geonode/uploaded
  sudo chown -Rf tomcat:www-data /opt/geonode/geonode/uploaded
  sudo chmod -Rf 777 /opt/geonode/geonode/uploaded/

  sudo touch /opt/geonode/geonode/.celery_results
  sudo chmod 777 /opt/geonode/geonode/.celery_results

  # Enable GeoNode NGINX config
  sudo ln -s /etc/nginx/sites-available/geonode /etc/nginx/sites-enabled/geonode

  # Restart the services
  sudo service tomcat9 restart
  sudo service nginx restart


Update the settings in order to use the ``PostgreSQL`` Database
...............................................................

.. warning:: Make sure you already installed and configured the Database as explained in the previous sections.

.. note:: Instead of using the ``local_settings.py``, you can drive the GeoNode behavior through the ``.env*`` variables; see as an instance the file ``./paver_dev.sh`` or ``./manage_dev.sh`` in order to understand how to use them. In that case **you don't need to create** the ``local_settings.py`` file; you can just stick with the decault one, which will take the values from the ENV. We tend to prefer this method in a production/dockerized system.

.. code-block:: shell

  workon geonode
  cd /opt/geonode

  # Initialize GeoNode
  chmod +x *.sh
  ./paver_local.sh reset
  ./paver_local.sh setup
  ./paver_local.sh sync
  ./manage_local.sh collectstatic --noinput
  sudo chmod -Rf 777 geonode/static_root/ geonode/uploaded/

Before finalizing the configuration we will need to update the ``UWSGI`` settings

Restart ``UWSGI`` and update ``OAuth2`` by using the new ``geonode.settings``

.. code-block:: shell

  # As superuser
  sudo su

  # Restart Tomcat
  service tomcat9 restart

  # Restart UWSGI
  pkill -9 -f uwsgi
  service uwsgi restart

  # Update the GeoNode ip or hostname
  cd /opt/geonode

  # This must be done the first time only
  cp package/support/geonode.binary /usr/bin/geonode
  cp package/support/geonode.updateip /usr/bin/geonode_updateip
  chmod +x /usr/bin/geonode
  chmod +x /usr/bin/geonode_updateip

  # Refresh GeoNode and GeoServer OAuth2 settings
  source .env_local
  PYTHONWARNINGS=ignore VIRTUAL_ENV=$VIRTUAL_ENV DJANGO_SETTINGS_MODULE=geonode.settings GEONODE_ETC=/opt/geonode/geonode GEOSERVER_DATA_DIR=/opt/data/geoserver_data TOMCAT_SERVICE="service tomcat9" APACHE_SERVICE="service nginx" geonode_updateip -p localhost

  # Go back to standard user
  exit

Check for any error with

.. code-block:: shell

  sudo tail -F -n 300 /var/log/uwsgi/app/geonode.log

Reload the UWSGI configuration with

.. code-block:: shell

  touch /opt/geonode/geonode/wsgi.py
