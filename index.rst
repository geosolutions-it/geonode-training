.. _mainindex:

==============================
GeoNode Training Documentation
==============================

Welcome to the GeoNode Training Documentation.

From here you can browse the documentation relative to the GeoNode Training Documentation.

.. hint:: An Ubuntu 18.04 Desktop 64-bit based Live ISO, containing the base packages and data for the execution of the workshop exercises, is available for download `here <https://goo.gl/ynKhuV>`_
    The Virtual Machine can also be launched through `VirtualBox <http://download.virtualbox.org/virtualbox/5.2.14/>`_ binaries.
    
    .. note:: The ISO should be mounted ad Live CD and the Virtual Machine must enable EFI Boot options
    
    The users to be used with the Virtual Machine are:
    
    .. code-block:: yaml
    
        geo/geo # principal user whith suoders privileges
        /home/geo/ # is the location where you can find sources, binaries and training data
        /home/geo/my_geonode # is the location of a materialized `geonode-project <https://github.com/geosolutions-it/geonode-project.git>`_ Django template
        /home/geo/Envs/geonode/src/geonode # is the location of the GeoNode core sources
        postgres/postgres # system user for the management of the DBMS
        

The Documentation is divided in six main sections:

.. hint:: The whole documentation for offline reading as a ZIP archive can be donwloaded `here <https://goo.gl/Z2fh4i>`_. The official GeoNode 2.8 documentation is also available for offline reading as a ZIP `here <https://goo.gl/Unxcq2>`_.

.. toctree::
    :hidden:
    
    001_overview_and_ref/index
    002_install_and_admin/index
    003_users_workshop/index
    004_admin_workshop/index
    005_dev_workshop/index
    006_adv_workshop/index

:ref:`overview_and_ref`
    This module guides the user to an overview of GeoNode and its main components.

    At the end of this section you will have a clear view of what GeoNode is and can do.
    You will be able also to use the GeoNode main functionalities and understand some of
    the basic concepts of the system infrastructure.

:ref:`installation_and_admin`
    This module is more oriented to users having some System Administrator background.

    At the end of this section you will be able to setup from scratch the whole GeoNode infrastructure and understand how to the different pieces are interconnected and which are their dependencies.

    *Prerequisites*
        Before proceeding with the reading, it is strongly recommended to be sure having clear the following concepts:

        1. GeoNode and Django framework basic concepts
        2. What is Python
        3. What is a DBMS
        4. What is a Java Virtual Machine and the JDK
        5. Linux OS basic shell and maintenance commands
        6. Basic TCP/IP and networking concepts
        7. Apache HTTPD Server and WSGI Python bindings

:ref:`users_workshop`
    This workshop will teach how to use the GeoNode going in depth into what we can do with software application.
    At the end of this section you will master all the GeoNode sections and entities from a user perspective. 

    You will know how to:

    1. Manage users accounts and how to modify them.
    2. Use and manage the different GeoNode basic resouces.
    3. Use the GeoNode searching tools to find your resources.
    4. Manage Layers and Maps, update the styles and publish them.
    5. Load datasets into GeoNode and keep them synchronized with GeoServer.

    *Prerequisites*
        Before proceeding with the reading, it is strongly recommended to be sure having clear the following concepts:

        1. GeoNode and Django framework basic concepts
        2. What is Python
        3. What is a geospatial server and a basic knowledge of the geospatial web services.
        4. What is a metadata and a catalog.
        5. What is a map and a legend.

:ref:`admin_workshop`
    This workshop will teach how to install and manage a deployment of the `GeoNode <http://geonode.org/>`_ software application.
    At the end of this section you will master all the GeoNode sections and entities from an administrator perspective. 

    You will know how to:

    1. Use the GeoNode’s Django Administration Panel.
    2. Use the console Management Commands for GeoNode.
    3. Configure and customize your GeoNode installation.

    *Prerequisites*
        Before proceeding with the reading, it is strongly recommended to be sure having clear the following concepts:

        1. GeoNode and Django framework concepts
        2. Good knowledge of Python
        3. Good knowledge of what is a geospatial server and geospatial web services.
        4. Good knowledge of what is metadata and catalog.
        5. Good knowledge of HTML and CSS.

:ref:`dev_workshop`
    This workshop will teach how to develop with and for the `GeoNode <http://geonode.org>`_ software application.
    This module will introduce you to the components that GeoNode is built with, the standards that it supports and the services it provides based on those standards, and an overview its architecture.

    *Prerequisites*
        GeoNode is a web based GIS tool, and as such, in order to do development on GeoNode itself or to integrate it into your own application, you should be familiar with basic web development concepts as well as with general GIS concepts.

:ref:`adv_workshop`
    This module introduces advanced tecquinques and metodologies for the management of the geospatial data and the maintenance and tuning of the servers on *Production Environments*.
    
    The last sections of the module will teach also you how to add brand new classes and functionalities to your GeoNode installation.
    
    *Prerequisites*
        You should be familiar with GeoNode, GeoServer, Python framework and development concepts other than with system administrator and caching concepts and tecnquiques.

Editing the documentation
-------------------------

This documentation is written in `reStructuredText <https://en.wikipedia.org/wiki/ReStructuredText>`_ format
and automatically build from `this GitHub repository <https://github.com/geosolutions-it/doc-geonode>`_
To edit the documentation you'll need the following tools:

- `Git <http://en.wikipedia.org/wiki/Git_(software)>`_
- `Python <https://www.python.org/>`_ and `pip <https://en.wikipedia.org/wiki/Pip_(package_manager)>`_ (recent versions of Python come bundled with pip)
- `Sphinx <http://sphinx-doc.org/index.html>`_

For installation and basic usage of the tools follow `these <./install-doc-tools.html>`_ instructions

License Information
-------------------

Documentation
.............

Documentation is released under a :term:`Creative Commons` license with the following conditions.

You are free to Share (to copy, distribute and transmit) and to Remix (to adapt) the documentation under the following conditions:

- Attribution. You must attribute the documentation to the author.

- Share Alike. If you alter, transform, or build upon this work, you may distribute the resulting work only under the same or similar license to this one.

With the understanding that:

- Any of the above conditions can be waived if you get permission from the copyright holder.

- Public Domain. Where the work or any of its elements is in the public domain under applicable law, that status is in no way affected by the license.

Other Rights. In no way are any of the following rights affected by the license:

- Your fair dealing or fair use rights, or other applicable copyright exceptions and limitations;

- The author's moral rights;

- Rights other persons may have either in the work itself or in how the work is used, such as publicity or privacy rights.

Notice: For any reuse or distribution, you must make clear to others the license terms of this work. The best way to do this is with a link to this web page.

You may obtain a copy of the License at `Creative Commons Attribution-ShareAlike 3.0 Unported License <http://creativecommons.org/licenses/by-sa/3.0/>`_

The document is written in reStructuredText format for consistency and portability.

Author Information
..................

This documentation was written by GeoSolutions.

The layout for the reStructuredText based documentation is based on the work done by the `GeoNode <http://geonode.org/>`_ project and the `Sphinx <http://sphinx.pocoo.org/>`_ framework.

If you have questions, found a bug or have enhancements, please contact us through info@geosolutions.it

.. glossary::

   Creative Commons
      `Creative Commons Attribution-ShareAlike 3.0 Unported License <http://creativecommons.org/licenses/by-sa/3.0/>`_
      Creative Commons (CC) is a non-profit organization devoted to
      expanding the range of creative works available for others to build
      upon legally and to share. The organization has released several
      copyright-licenses known as Creative Commons licenses free of charge
      to the public. These licenses allow creators to communicate which
      rights they reserve, and which rights they waive for the benefit of
      recipients or other creators. An easy-to-understand one-page
      explanation of rights, with associated visual symbols, explains the
      specifics of each Creative Commons license. Creative Commons licenses
      do not replace copyright, but are based upon it. They replace
      individual negotiations for specific rights between copyright owne
      (licensor) and licensee, which are necessary under an "all rights
      reserved" copyright management, with a "some rights reserved"
      management employing standardized licenses for re-use cases where no
      commercial compensation is sought by the copyright owner. The result
      is an agile, low-overhead and low-cost copyright-management regime,
      profiting both copyright owners and licensees.
