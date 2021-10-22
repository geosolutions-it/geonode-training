.. _dependencies:

===========================
GeoNode System Dependencies
===========================

Overview
========

The following steps will guide you to a fresh setup of GeoNode.

All guides will first install and configure the system to run it in ``DEBUG`` mode (also known as ``DEVELOPMENT`` mode)
and then by configuring an HTTPD server to serve GeoNode through the standard ``HTTP`` (``80``) port.

.. warning:: Those guides **are not** meant to be used on a production system.
  There will be dedicated chapters that will show you some *hints* to optimize GeoNode for a production-ready machine.
  In any case, we strongly suggest to task an experienced *DevOp* or *System Administrator* before exposing your server to the ``WEB``.

Ubuntu 20.04LTS
===============

This part of the documentation describes the complete setup process for GeoNode on an Ubuntu 20.04LTS **64-bit** clean environment (Desktop or Server).

All examples use shell commands that you must enter on a local terminal or a remote shell.

- If you have a graphical desktop environment you can open the terminal application after login;
- if you are working on a remote server the provider or sysadmin should has given you access through an ssh client.

.. _install_dep:

1. Install the dependencies
^^^^^^^^^^^^^^^^^^^^^^^^^^^

In this section, we are going to install all the basic packages and tools needed for a complete GeoNode installation.

.. warning:: To follow this guide, a basic knowledge about Ubuntu Server configuration and working with a shell is required.

.. note:: This guide uses ``vim`` as the editor; fill free to use ``nano``, ``gedit`` or others.

Upgrade system packages
.......................

Check that your system is already up-to-date with the repository running the following commands:

.. code-block:: shell

   sudo add-apt-repository ppa:ubuntugis/ubuntugis-unstable
   sudo apt update -y; sudo apt upgrade -y;


Packages Installation
.....................

.. note:: You don't need to install the **system packages** if you want to run the project using Docker

We will use **example.org** as fictitious Domain Name.

First, we are going to install all the **system packages** needed for the GeoNode setup. Login to the target machine and execute the following commands:

.. code-block:: shell

  # Install packages from GeoNode core
  sudo apt install -y build-essential gdal-bin \
      python3.8-dev python3.8-venv virtualenvwrapper \
      libxml2 libxml2-dev gettext vim \
      libxslt1-dev libjpeg-dev libpng-dev libpq-dev libgdal-dev \
      software-properties-common build-essential \
      git unzip gcc zlib1g-dev libgeos-dev libproj-dev \
      sqlite3 spatialite-bin libsqlite3-mod-spatialite libsqlite3-dev

  # Install Openjdk
  sudo apt install openjdk-8-jdk-headless default-jdk-headless -y

  # Verify GDAL version
  gdalinfo --version
    $> GDAL 3.3.2, released 2021/09/01

  # Verify Python version
  python3.8 --version
    $> Python 3.8.10

  which python3.8
    $> /usr/bin/python3.8

  # Verify Java version
  java -version
    $> openjdk version "1.8.0_292"
    $> OpenJDK Runtime Environment (build 1.8.0_292-8u292-b10-0ubuntu1~20.04-b10)
    $> OpenJDK 64-Bit Server VM (build 25.292-b10, mixed mode)

  # Cleanup the packages
  sudo apt update -y; sudo apt upgrade -y; sudo apt autoremove --purge

.. warning:: GeoNode 3.x is not compatible with Python < 3.7
