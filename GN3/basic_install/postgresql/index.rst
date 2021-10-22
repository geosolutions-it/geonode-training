.. _postgres:

======================
Postgis database Setup
======================

.. warning::

  Be sure you have successfully completed all the steps of the section :ref:`dependencies`.

In this section, we are going to setup users and databases for GeoNode in PostgreSQL.

Install and Configure the PostgreSQL Database System
....................................................

In this section we are going to install the ``PostgreSQL`` packages along with the ``PostGIS`` extension. Those steps must be done **only** if you don't have the DB already installed on your system.

.. code-block:: shell

  # Ubuntu 20.04 (focal)
  sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
  sudo wget --no-check-certificate --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
  sudo apt update -y; sudo apt install -y postgresql-13 postgresql-13-postgis-3 postgresql-13-postgis-3-scripts postgresql-13 postgresql-client-13

We now must create two databases, ``geonode`` and ``geonode_data``, belonging to the role ``geonode``.

.. warning:: This is our default configuration.
  You can use any database or role you need.
  The connection parameters must be correctly configured on ``settings``, as we will see later in this section.

Databases and Permissions
.........................

First, create the geonode user. GeoNode is going to use this user to access the database

.. code-block:: shell

  sudo service postgresql start
  sudo -u postgres createuser -P geonode

  # Use the password: geonode

You will be prompted asked to set a password for the user. **Enter geonode as password**.

.. warning:: This is a sample password used for the sake of simplicity. This password is very **weak** and should be changed in a production environment.

Create database ``geonode`` and ``geonode_data`` with owner ``geonode``

.. code-block:: shell

  sudo -u postgres createdb -O geonode geonode
  sudo -u postgres createdb -O geonode geonode_data

Next let's create PostGIS extensions

.. code-block:: shell

  sudo -u postgres psql -d geonode -c 'CREATE EXTENSION postgis;'
  sudo -u postgres psql -d geonode -c 'GRANT ALL ON geometry_columns TO PUBLIC;'
  sudo -u postgres psql -d geonode -c 'GRANT ALL ON spatial_ref_sys TO PUBLIC;'
  sudo -u postgres psql -d geonode -c 'GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO geonode;'
  sudo -u postgres psql -d geonode -c 'GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO geonode;'

  sudo -u postgres psql -d geonode_data -c 'CREATE EXTENSION postgis;'
  sudo -u postgres psql -d geonode_data -c 'GRANT ALL ON geometry_columns TO PUBLIC;'
  sudo -u postgres psql -d geonode_data -c 'GRANT ALL ON spatial_ref_sys TO PUBLIC;'
  sudo -u postgres psql -d geonode_data -c 'GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO geonode;'
  sudo -u postgres psql -d geonode_data -c 'GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO geonode;'

Final step is to change user access policies for local connections in the file ``pg_hba.conf``

.. code-block:: shell

  sudo vim /etc/postgresql/13/main/pg_hba.conf

Scroll down to the bottom of the document. We want to make local connection ``trusted`` for the default user.

Make sure your configuration looks like the one below.

.. code-block:: shell

    ...
    # DO NOT DISABLE!
    # If you change this first entry you will need to make sure that the
    # database superuser can access the database using some other method.
    # Noninteractive access to all databases is required during automatic
    # maintenance (custom daily cronjobs, replication, and similar tasks).
    #
    # Database administrative login by Unix domain socket
    local   all             postgres                                trust

    # TYPE  DATABASE        USER            ADDRESS                 METHOD

    # "local" is for Unix domain socket connections only
    local   all             all                                     md5
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5
    # IPv6 local connections:
    host    all             all             ::1/128                 md5
    # Allow replication connections from localhost, by a user with the
    # replication privilege.
    local   replication     all                                     peer
    host    replication     all             127.0.0.1/32            md5
    host    replication     all             ::1/128                 md5

.. warning:: If your ``PostgreSQL`` database resides on a **separate/remote machine**, you'll have to **allow** remote access to the databases in the ``/etc/postgresql/13/main/pg_hba.conf`` to the ``geonode`` user and tell PostgreSQL to **accept** non-local connections in your ``/etc/postgresql/13/main/postgresql.conf`` file

Restart PostgreSQL to make the change effective.

.. code-block:: shell

  sudo service postgresql restart

PostgreSQL is now ready. To test the configuration, try to connect to the ``geonode`` database as ``geonode`` role.

.. code-block:: shell

  psql -U postgres geonode
  # This should not ask for any password

  psql -U geonode geonode
  # This should ask for the password geonode

  # Repeat the test with geonode_data DB
  psql -U postgres geonode_data
  psql -U geonode geonode_data
