.. _geonode:

=====================
GeoNode Basic Install
=====================

This is the most basic installation of GeoNode. It won't use any external server like ``Apache Tomcat``, ``PostgreSQL`` or ``HTTPD``.

First of all we need to prepare a new Python Virtual Environment

Since geonode needs a large number of different python libraries and packages, its recommended to use a python virtual environment to avoid conflicts on dependencies with system wide python packages and other installed software. See also documentation of `Virtualenvwrapper <https://virtualenvwrapper.readthedocs.io/en/stable/>`_ package for more information

.. note:: The GeoNode Virtual Environment must be created only the first time. You won't need to create it again everytime.

.. code-block:: shell

  which python3.8  # copy the path of python executable

  # Create the GeoNode Virtual Environment (first time only)
  export WORKON_HOME=~/.virtualenvs
  source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
  mkvirtualenv --python=/usr/bin/python3.8 geonode  # Use the python path from above

  # Alterantively you can also create the virtual env like below
  mkdir -p ~/.virtualenvs
  python3.8 -m venv ~/.virtualenvs/geonode
  source ~/.virtualenvs/geonode/bin/activate


At this point your command prompt shows a ``(geonode)`` prefix, this indicates that your virtualenv is active.

.. note:: The next time you need to access the Virtual Environment just run

  .. code-block:: shell

    source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
    workon geonode

    # Alterantively you can also create the virtual env like below
    source ~/.virtualenvs/geonode/bin/activate

.. note:: In order to save permanently the virtualenvwrapper environment

  .. code-block:: shell

    vim ~/.bashrc

    # Write to the bottom of the file the following lines
    export WORKON_HOME=~/.virtualenvs
    source /usr/share/virtualenvwrapper/virtualenvwrapper.sh

.. code-block:: shell

  # Let's create the GeoNode core base folder and clone it
  sudo mkdir -p /opt/geonode/; sudo usermod -a -G www-data $USER; sudo chown -Rf $USER:www-data /opt/geonode/; sudo chmod -Rf 775 /opt/geonode/

  # Clone the GeoNode source code on /opt/geonode
  cd /opt; git clone https://github.com/GeoNode/geonode.git -b 3.2.1 geonode

.. code-block:: shell

  # Install the Python packages
  cd /opt/geonode
  pip install -r requirements.txt --upgrade --no-cache --no-cache-dir
  pip install -e . --upgrade
  pip install pygdal=="`gdal-config --version`.*"

Initializae GeoNode

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
