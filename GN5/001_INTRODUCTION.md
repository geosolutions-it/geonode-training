# Introduction on GeoNode MapStore client

The [geonode-mapstore-client](https://github.com/GeoNode/geonode-mapstore-client/tree/3.2.x) is a downstream project of [MapStore](https://github.com/geosolutions-it/MapStore2). GeoNode uses MapStore as a frontend framework to include map/layers viewers and editors or application related to the concept of spatial data.

The geonode-mapstore-client respository provides two main modules:

- **geonode_mapstore_client**: it contains html templates, hookset, javascript source files and static files to enable the MapStore applications to be available inside the GeoNode pages 
- **mapstore2_adapter**: it contains functions to convert MapStore data to GeoNode resource and vice versa (eg map configuration)

We will focus on the **geonode_mapstore_client** module that exposes all the mapstore application

Below some useful links about the MapStore project:

- [MapStore documentation](https://mapstore.readthedocs.io/en/latest/)
- [MapStore plugins documentation](https://mapstore.geosolutionsgroup.com/mapstore/docs/api/plugins)
- [MapStore framework documentation](https://mapstore.geosolutionsgroup.com/mapstore/docs/api/framework)
