# Adding Data to GeoNode

## Introduction to OGC
From the [OGC home page](http://www.opengeospatial.org/):

*The OGC (Open Geospatial Consortium) is an international not for profit organization committed to making quality open standards for the global geospatial community. These standards are made through a consensus process and are freely available for anyone to use to improve sharing of the world’s geospatial data.*

*OGC* provides standard for both data and protocols to exchange it.

### Data standards
Some example of standards at the data level include:

 * `GeoTIFF`, is a public domain metadata standard which allows georeferencing information to be embedded within a TIFF file. The potential additional information includes map projection, coordinate systems, ellipsoids, datums, and everything else necessary to establish the exact spatial reference for the file. The GeoTIFF format is fully compliant with TIFF 6.0, so software incapable of reading and interpreting the specialized metadata will still be able to open a GeoTIFF format file.
 * `KML` (Keyhole Markup Language), a XML dialect used to exchange mostly vector data and control its visualization.
 * `Shapefile`, is a geospatial vector data format for geographic information system (GIS) software. It is developed and regulated by Esri as a mostly open specification for data interoperability among Esri and other GIS software products. The shapefile format can spatially describe vector features: points, lines, and polygons, representing, for example, water wells, rivers, and lakes. Each item usually has attributes that describe it, such as name or temperature.
 * `GeoJSON`, is an open standard format designed for representing simple geographical features, along with their non-spatial attributes. It is based on the JSON format.

GeoNode supports the above as either inputs or outputs.

### Styling standards
OGC has created `SLD` (Styled Layer Descriptor) to define an encoding that extends the Web Map Service (`WMS`) standard to allow user-defined symbolization as a styling rules of geographic features and created `SE` (Symbology Encoding) as an styling language that the client and server can both understand. See the Pretty maps with GeoNode section for further details.

### Protocol standards
Protocols define how network communication must take place between two systems. The OGC has defined several protocols classified appropriately by INSPIRE in the following classes:

  * Discovery services
  * View services
  * Download services
  * Transformation services

![image](https://user-images.githubusercontent.com/1278021/125443072-80ceacc2-946f-445a-9b41-9ea2edc1d02b.png)

The idea is simple. A user would first use a *Discovery service* to locate the data of interest, then would use a *View service* for a visual inspection. Once satisfied, eventually to download the data for offline usage, or to use a *Transformation service* to perform an online spatial analysis.

**Discovery** services are implemented using `CSW` (Catalog Services for the Web), a protocol designed with a standards-based interface to discover, browse, and query metadata of geospatial data. An interaction with a `CSW` (e.g., [pycsw](http://pycsw.org)) typically leads to find links to other `OGC` protocols to view and download the data. GeoNode has a `CSW` module that’s allows to search for datasets and contents.

**View** services are implemented using `WMS` (Web Map Service) and `WMTS` (Web Map Tile Service), a stanard `HTTP` protocols that allow a client to view maps images from one or more distributed geospatial databases. The latter are the easiest services to use and understand, they simply provide a list of maps that can be displayed and combined in a browser application, eventually queried, but not modified or changed. You can learn more about `WMS` in the next sections.

**Download** services are implemented using `WFS` (Web Feature Service) and `WCS` (Web Coverage Service, protocols allow downloading the source data behind maps, either in *vector* (`WFS`) or *raster* (`WCS`) formats, allowing to perform the following extra actions during the download:

  * Spatial filtering (e.g., by bounding box), thus extracting only a certain subset of the data;
  * Alphanumeric filtering (only `WFS`), or extraction by time (also `WCS`);
  * Selection of attributes of interest (`WFS`) or bands (`WCS`);
  * Change of map projection;
  * Rescaling (only for `WCS`, e.g., allows to download a version of raster at a lower resolution to save on bandwidth).

**Transformation** services are used to perform online spatial analysis on vector and raster data. The protocol used in this case is `WPS` (Web Processing Service).

### Common requests in protocol standards
A client makes requests in order to interact with a service. Each request has a different role also different services show different calls, but there are a few common points.

Here are common requests and their meaning:

  * **GetCapabilities**, this is the only request present in all protocols. It returns a long `XML` document describing what the server can do and what it contains. It allows the client to discover which parts of a protocol are actually supported (some can be optional), which formats are supported in return, and which “subjects” or “contents” are available in the server (might be maps, vector data, raster data, and so on)
  * **Describe< Content>**, present in some protocols. It allows to get some further information about a particular content delivered by the server. For example, `DescribeFeatureType` in `WFS` provides the list of attributes for a particular vector layer, while `DescribeProcess` in `WPS` describes inputs and outputs of a spatial analysis process.
  * **Get< Content>**, this call retrieves the particular content, normally providing extra parameters to control the production/extraction of the output. For example, we have `GetMap` in `WMS`, `GetFeature` in `WFS`, and `GetCoverage` in `WCS`.

These requests are normally performed in sequence by the client. Each one enables the usage of the next one.

![image](https://user-images.githubusercontent.com/1278021/125444146-365b6772-5c3a-4fc7-b853-bae06cbc589c.png)

### Ways to encode requests
Requests in `OGC` protocols can normally be performed using two different paradigms:

  * **KVP** or Key Value Pair. It’s a simple URL that supports all the requests parameters as pairs of keys and values in the query string. The `HTTP` request is a `GET` in this case. Here is an example of a `WFS GetFeature` in `KVP` mode

```shell
  http://demo.geo-solutions.it/geoserver/wfs?
    request=GetFeature&
    version=1.1.0&
    typeName=topp:states&
    propertyName=STATE_NAME,PERSONS&
    BBOX=-75.102613,40.212597,-72.361859,41.512517,EPSG:4326
```

  * **XML POST**, contains a `XML` document describing the request and sent to the server as a `HTTP POST` request. Here is an example of a `WFS GetFeature` using `HTTP POST` mode

```xml
  <wfs:GetFeature service="WFS" version="1.0.0"
    outputFormat="GML2"
    xmlns:topp="http://www.openplans.org/topp"
    xmlns:wfs="http://www.opengis.net/wfs"
    xmlns:ogc="http://www.opengis.net/ogc"
    xmlns:gml="http://www.opengis.net/gml"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.opengis.net/wfs
                        http://schemas.opengis.net/wfs/1.0.0/WFS-basic.xsd">
    <wfs:Query typeName="topp:states">
      <ogc:PropertyName>topp:STATE_NAME</ogc:PropertyName>
      <ogc:PropertyName>topp:PERSONS</ogc:PropertyName>
      <ogc:Filter>
        <ogc:BBOX>
          <ogc:PropertyName>the_geom</ogc:PropertyName>
          <gml:Box srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
             <gml:coordinates>-75.102613,40.212597 -72.361859,41.512517</gml:coordinates>
          </gml:Box>
        </ogc:BBOX>
     </ogc:Filter>s
    </wfs:Query>
  </wfs:GetFeature>
```

*GeoNode aims to simplify the process of publishing geospatial contents to the server and seamplessly expose them through the OGC Standard protocols*.

In the next section we will learn how to publish basic geospatial data types into GeoNode.

#### [Next Section: Adding base types](ADDING_BASE_TYPES.md)
