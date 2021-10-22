.. _docker:

===========================
GeoNode Docker Installation
===========================

Overview
========

The preferred way to deploy GeoNode is via Docker containers. The following section will guide on how to accomplish it.

This section assumes that you are aware of Docker and containerization technology.
For further information please visit `<https://docs.docker.com/get-started/overview/>`_

Docker Setup
============

.. code-block:: shell

  # install OS level packages..
  sudo add-apt-repository universe
  sudo apt-get update -y
  sudo apt-get install -y git-core git-buildpackage debhelper devscripts python3.8-dev python3.8-venv virtualenvwrapper
  sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common vim

  # add docker repo and packages...
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

  sudo apt-get update -y
  sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose
  sudo apt autoremove --purge

  # add your user to docker group...
  sudo usermod -aG docker ${USER}
  su ${USER}

Create an instance of your ``geonode-project``
----------------------------------------------

**NOTE: You can call your geonode project whatever you like except 'geonode'. Follow the naming conventions for python packages (generally lower case with underscores (_).**

Let's say your project is named :guilabel:`my_geonode` perform the following steps:

.. code-block:: shell

  git clone https://github.com/GeoNode/geonode-project.git -b 3.2.x

  # Ubuntu
  source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
  mkvirtualenv --python=/usr/bin/python3 my_geonode

  Alterantively you can also create the virtual env like below
  python3.8 -m venv /home/geonode/dev/.venvs/my_geonode
  source /home/geonode/dev/.venvs/my_geonode/bin/activate

  pip install Django==3.2

  django-admin startproject --template=./geonode-project -e py,sh,md,rst,json,yml,ini,env,sample,properties -n monitoring-cron -n Dockerfile my_geonode

  cd my_geonode


Set Environment Variables Create a .env file using the available .env.sample
Assign values to the following variables in the .env:

.. code-block:: shell

      OAUTH2_CLIENT_ID
      OAUTH2_CLIENT_SECRET
      SECRET_KEY (a random one will be generated at project creation)
      DEFAULT_FROM_EMAIL



Startup the containers
----------------------

.. code-block:: shell

  docker-compose build --no-cache
  set COMPOSE_CONVERT_WINDOWS_PATHS=1
  docker-compose up -d

  # On first install db migrations have to made...this will take a while
  # Check django container logs while you wait...

  docker-compose logs -f django


- If any error occurs, try to catch the error stacktrace by running the following commands from ``my_geonode`` root folder:

    .. code-block:: shell

        # GeoNode “entrypoint.sh” Logs
        tail -F -n 300 invoke.log


Connect to :guilabel:`http://localhost/`
----------------------------------------

The startup typically takes some time, so be patient…

If everything goes well, you should be able to see from the ``geonode startup logs`` a line similar to the following one:

.. code-block:: shell

  <some date> [UWSGI] Uwsgi running...

Connect to :guilabel:`http://localhost/`

The default credentials are:

 * GeoNode (:guilabel:`http://localhost/`) :guilabel:`admin`:

    ``username: admin``
    ``password: admin``

 * GeoServer (:guilabel:`http://localhost/geoserver/`) :guilabel:`admin`:

    ``username: admin``
    ``password: geoserver``


Run the instance in development mode
------------------------------------

Use dedicated docker-compose files while developing

**NOTE: In this example we are going to keep localhost as the target IP for GeoNode**

.. code-block:: shell

    docker-compose -f docker-compose.development.yml -f docker-compose.development.override.yml up


Deploy GeoNode on a production server (e.g.: http://my_geonode.geonode.org/)
============================================================================

In the case you would like to deploy to, let's say, :guilabel:`http://my_geonode.geonode.org/`, you will need to change ``.env`` as follows:

.. code-block:: shell

  # backup original .env file
  cp .env .env.bak
  vim .env
  --> replace http://localhost with http://my_geonode.geonode.org everywhere (:%s/localhost/my_geonode.geonode.org/g)
  vim /etc/hosts
  --> create an alias to your localhost: 127.0.0.1	my_geonode.geonode.org

Restart the containers
----------------------

Whenever you change someting on :guilabel:`.env` file, you will need to rebuild the container

.. warning:: **Be careful!** The following command drops any change you might have done manually inside the containers, except for the static volumes.

.. code-block:: shell

  docker-compose up --build -d

Troubleshooting
---------------

If for some reason you are not able to reach the server on the :guilabel:`HTTPS` channel, please check the :guilabel:`NGINX` configuration files below:

1. Enter the :guilabel:`NGINX` container

    .. code-block:: shell

      docker-compose exec geonode sh

2. Install an editor if not present

    .. code-block:: shell

      apk add nano

3. Double check that the ``nginx.https.enabled.conf`` link has been correctly created

    .. code-block:: shell

      ls -lah

    .. figure:: img/throubleshooting_prod_001.png
        :align: center

    If the list does not match exactly the figure above, please run the following commands, and check again

    .. code-block:: shell

      rm nginx.https.enabled.conf
      ln -s nginx.https.available.conf nginx.https.enabled.conf

4. Inspect the ``nginx.https.enabled.conf`` contents

    .. code-block:: shell

      nano nginx.https.enabled.conf

    Make sure the contents match the following

    .. warning::

      Change the :guilabel:`Hostname` accordingly. **This is only an example!**

    .. code-block:: shell

        # NOTE : $VARIABLES are env variables replaced by entrypoint.sh using envsubst
        # not to be mistaken for nginx variables (also starting with $, but usually lowercase)

        # This file is to be included in the main nginx.conf configuration if HTTPS_HOST is set
        ssl_session_cache   shared:SSL:10m;
        ssl_session_timeout 10m;

        # this is the actual HTTPS host
        server {
            listen              443 ssl;
            server_name         my_geonode.geonode.org;
            keepalive_timeout   70;

            ssl_certificate     /certificate_symlink/fullchain.pem;
            ssl_certificate_key /certificate_symlink/privkey.pem;
            ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
            ssl_ciphers         HIGH:!aNULL:!MD5;

            include sites-enabled/*.conf;
        }

        # if we try to connect from http, we redirect to https
        server {
            listen 80;
            server_name  my_geonode.geonode.org; # TODO : once geoserver supports relative urls, we should allow access though both HTTP and HTTPS at the same time and hence remove HTTP_HOST from this line

            # Except for let's encrypt challenge
            location /.well-known {
                alias /geonode-certificates/.well-known;
                include  /etc/nginx/mime.types;
            }

            # Redirect to https
            location / {
            return 302 https://my_geonode.geonode.org/$request_uri; # TODO : we should use 301 (permanent redirect, but not practical for debug)
            }
        }

    .. warning::

      **Save the changes, if any, and exit!**

5. Reload the NGINX configuration

    .. code-block:: shell

      nginx -s reload
      2020/06/24 10:00:11 [notice] 112#112: signal process started
      /etc/nginx# exit

6. It may be helpful to disable https to isolate the source of errors. After reverting the HTTPS-related changes in the `.env` file, repeat the above steps and ensure that the ``nginx.http.enabled.conf`` link has been correctly created.

    .. code-block:: shell

      ln -s nginx.conf nginx.http.enabled.conf
      nano nginx.http.enabled.conf


Customize :guilabel:`.env` to match your needs
==============================================

In the case you would like to modify the GeoNode behavior, always use the :guilabel:`.env` file in order to update the :guilabel:`settings`.

If you need to change a setting which does not exist in :guilabel:`.env`, you can force the values inside :guilabel:`my_geonode/settings.py`

You can add here any property referred as

    | Env: ``PROPERTY_NAME``


Restart the containers
----------------------

Whenever you change someting on :guilabel:`.env` file, you will need to rebuild the containers.

.. warning:: **Be careful!** The following command drops any change you might have done manually inside the containers, except for the static volumes.

.. code-block:: shell

  docker-compose up -d django
