# Preparation of the Dev Environment

## Start GeoNode in DEV Mode
In this section we will learn how to run GeoNode in _development mode_; this particular way to run GeoNode will allow us to view and debug any change to the code at runtime, without the need to restart the services.

Notice that we will still need the following services to run on the system:

 - **PostgreSQL service w/ Portgis extensions**; we will learn how to link Dev GeoNode to an existing database and how to initialize a new one.
 - **Apache Tomcat9 with GeoServer**; we will still need a running GeoServer instance to be able to manage the geospatial datasets.

