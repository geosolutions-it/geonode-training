# Pretty Maps with GeoNode

## Introduction to Web Map Service
A Web Map Service provides:

* A set of **layers** (each in its own map projection)
* One or more named **styles** per layer (at least one)
* Ability to **request any list of layers** with their styles to build a map
* A set of target **projections** in which the map can be composed
* Ability to ask “any” map size and scale denominator
* Optional ability to **query the map** (info tool, “what’s here?”)

![image](https://user-images.githubusercontent.com/1278021/125515287-1b4af3c4-39e5-4784-8e5a-3eccdefae522.png)

The flexibility of this model allows clients to build their own maps, selecting only the layers of interest, and integrating map layers from different servers (assuming there is at least one common target projection among the servers), and using local data too if needed.

This introduction showcases the `WMS 1.1` protocol version, as it’s easier to explain compared to the newer 1.3 version, in which a different axis order must be used depending on the requested coordinate reference system. The main concepts are very similar, and so are the requests.

### GetCapabilities
Like most `OGC` protocols, `WMS` exposes a `GetCapabilities` request. To see the full contents of the local `GeoServer WMS 1.3 capabilities` document follow this link:

```
http://localhost/geoserver/ows?service=wms&version=1.3.0&request=GetCapabilities
```

The result is a very large `XML` document, meant to be parsed by a computer, here we’ll look only at some highlights.

#### The service section
This section provides a way to describe the service contents (title, abstract) and points of contacts that can be used to inquire about the service, report issues and the link.
The contact information comes from the homonymous configuration page in the `Contact information` menu of the `GeoServer administration interface`.

```xml
<Service>
  <Name>WMS</Name>
  <Title>GeoNode Local GeoServer</Title>
  <Abstract>This is a description of your Web Map Server.</Abstract>
  <KeywordList>
    <Keyword>WFS</Keyword>
    <Keyword>WMS</Keyword>
    <Keyword>GEOSERVER</Keyword>
  </KeywordList>
  <OnlineResource xlink:type="simple" xlink:href="http://geoserver.sourceforge.net/html/index.php"/>
  <ContactInformation>
    <ContactPersonPrimary>
      <ContactPerson/>
      <ContactOrganization/>
    </ContactPersonPrimary>
    <ContactPosition/>
    <ContactAddress>
      <AddressType/>
      <Address/>
      <City/>
      <StateOrProvince/>
      <PostCode/>
      <Country/>
    </ContactAddress>
    <ContactVoiceTelephone/>
    <ContactFacsimileTelephone/>
    <ContactElectronicMailAddress/>
  </ContactInformation>
  <Fees>NONE</Fees>
  <AccessConstraints>NONE</AccessConstraints>
</Service>
```

#### The capability section
This section reports which requests the server can handle, and some information about the requests, such as the output formats. 

Did you know that:

* Not all requests are mandatory, only `GetCapability` and `GetMap` are, `GetFeatureInfo` is not
* Only one among `PNG`, `JPEG` and `GIF` `GetMap` output format is required, but look at how many GeoServer supports

```xml
<Capability>
    <Request>
      <GetCapabilities>
        <Format>text/xml</Format>
        <DCPType>
          <HTTP>
            <Get>
              <OnlineResource xlink:type="simple" xlink:href="http://localhost/geoserver/ows?SERVICE=WMS&amp;"/>
            </Get>
            <Post>
              <OnlineResource xlink:type="simple" xlink:href="http://localhost/geoserver/ows?SERVICE=WMS&amp;"/>
            </Post>
          </HTTP>
        </DCPType>
      </GetCapabilities>
      <GetMap>
        <Format>image/png</Format>
        <Format>application/atom+xml</Format>
        <Format>application/json;type=utfgrid</Format>
        <Format>application/pdf</Format>
        <Format>application/rss+xml</Format>
        <Format>application/vnd.google-earth.kml+xml</Format>
        <Format>application/vnd.google-earth.kml+xml;mode=networklink</Format>
        <Format>application/vnd.google-earth.kmz</Format>
        <Format>image/geotiff</Format>
        <Format>image/geotiff8</Format>
        <Format>image/gif</Format>
        <Format>image/jpeg</Format>
        <Format>image/png; mode=8bit</Format>
        <Format>image/svg+xml</Format>
        <Format>image/tiff</Format>
        <Format>image/tiff8</Format>
        <Format>image/vnd.jpeg-png</Format>
        <Format>image/vnd.jpeg-png8</Format>
        <Format>text/html; subtype=openlayers</Format>
        <Format>text/html; subtype=openlayers2</Format>
        <Format>text/html; subtype=openlayers3</Format>
        <DCPType>
          <HTTP>
            <Get>
              <OnlineResource xlink:type="simple" xlink:href="http://localhost/geoserver/ows?SERVICE=WMS&amp;"/>
            </Get>
          </HTTP>
        </DCPType>
      </GetMap>
      <GetFeatureInfo>
        <Format>text/plain</Format>
        <Format>application/vnd.ogc.gml</Format>
        <Format>text/xml</Format>
        <Format>application/vnd.ogc.gml/3.1.1</Format>
        <Format>text/xml; subtype=gml/3.1.1</Format>
        <Format>text/html</Format>
        <Format>application/json</Format>
        <DCPType>
          <HTTP>
            <Get>
              <OnlineResource xlink:type="simple" xlink:href="http://localhost/geoserver/ows?SERVICE=WMS&amp;"/>
            </Get>
          </HTTP>
        </DCPType>
      </GetFeatureInfo>
    </Request>
  ...
```

#### The Contents section
This section in `WMS` contains a tree of layers.

The tree has several consequences:

* It’s a way to organize data by themes
* It’s a way to group layers, asking for one layer makes the server return all its child layers, in the order they are defined.
* Allows inheritance, the properties of the parent layer are inherited by all child layers

The contents section starts with a root layer that can be used to get all layers in a single shot, and that lists all the target projections map can be reprojected into.
By default GeoServer lists all those found in the `EPSG` database, and it can be configured by setting the `Limited SRS list` in the GeoServer `WMS admin` page:

```xml
<Layer>
  <Title>GeoNode Local GeoServer</Title>
  <Abstract>This is a description of your Web Map Server.</Abstract>
  <!--Limited list of EPSG projections:-->
  <CRS>EPSG:4326</CRS>
  <CRS>EPSG:3785</CRS>
  <CRS>EPSG:3857</CRS>
  <CRS>EPSG:900913</CRS>
  <CRS>EPSG:32647</CRS>
  <CRS>EPSG:32736</CRS>
  <CRS>CRS:84</CRS>
  <EX_GeographicBoundingBox>
    <westBoundLongitude>-117.36526677636073</westBoundLongitude>
    <eastBoundLongitude>-105.00013888888888</eastBoundLongitude>
    <southBoundLatitude>32.534227762958466</southBoundLatitude>
    <northBoundLatitude>40.30013888888889</northBoundLatitude>
  </EX_GeographicBoundingBox>
  <BoundingBox CRS="CRS:84" minx="-117.36526677636073" miny="32.534227762958466" maxx="-105.00013888888888" maxy="40.30013888888889"/>
  <BoundingBox CRS="EPSG:4326" minx="32.534227762958466" miny="-117.36526677636073" maxx="40.30013888888889" maxy="-105.00013888888888"/>
  <BoundingBox CRS="EPSG:3785" minx="-1.3065041734361183E7" miny="3810657.25465308" maxx="-1.1688561994334111E7" maxy="4881998.79461361"/>
  <BoundingBox CRS="EPSG:3857" minx="-1.3065041734361183E7" miny="3833642.429924806" maxx="-1.1688561994334111E7" maxy="4909653.984025123"/>
  <BoundingBox CRS="EPSG:900913" minx="-1.3065041734361183E7" miny="3833642.429924806" maxx="-1.1688561994334111E7" maxy="4909653.984025123"/>
  <BoundingBox CRS="EPSG:32647" minx="-3.951956507952588E7" miny="1.9702860254844192E7" maxx="-9842223.141078195" maxy="8.402118175739984E7"/>
  <BoundingBox CRS="EPSG:32736" minx="5073132.765295756" miny="3.0483944181415446E7" maxx="2.9354901643614803E7" maxy="7.986815078653288E7"/>
  ...
```

After the list of coordinate reference systems, the actual layers show up with their own bounding box, styles, and legend reference:

```xml
<Layer queryable="1" opaque="0">
  <Name>geonode:Air_Runways</Name>
  <Title>Air_Runways</Title>
  <Abstract>Airport Runways</Abstract>
  <KeywordList>
    <Keyword>features</Keyword>
    <Keyword>Air_Runways</Keyword>
    <Keyword>Runway</Keyword>
    <Keyword>Airport</Keyword>
    <Keyword>Airports</Keyword>
    <Keyword>Runways</Keyword>
    <Keyword>Landing Strips</Keyword>
  </KeywordList>
  <CRS>EPSG:2230</CRS>
  <CRS>CRS:84</CRS>
  <EX_GeographicBoundingBox>
    <westBoundLongitude>-117.36526677636073</westBoundLongitude>
    <eastBoundLongitude>-116.12621141861304</eastBoundLongitude>
    <southBoundLatitude>32.534227762958466</southBoundLatitude>
    <northBoundLatitude>33.36158563104289</northBoundLatitude>
  </EX_GeographicBoundingBox>
  ...
```

This section allows the client to figure out which layers are available, where they are located, what coordinate reference systems can be used in requests, thus building the GetMap request.

### GetMap
The `GetMap` request allows a client to ask the server to generate a portryal of a dataset given a specific style (it will use the default one if none has been provided).

A `GetMap` request is a `HTTP GET` uri with a well-defined `query-string`; as an instance the following one is a typical `GetMap` request:

|Element   | Description|
|----------|------------|
|http://localhost/geoserver/geonode/wms? | The base URL|
|service=WMS|The service|
|version=1.1.0|The service version|
|request=GetMap|The request|
|layers=geonode:states|The list of layers (comma separated, can be one)|
|styles=|The list of style names (comma separated, can be empty to use the default)|
|bbox=-124.73142200000001, 24.955967,-66.969849, 49.371735|The area that should be depicted|
|width=768|Width of the output in pixels|
|height=330|Height of the output in pixels|
|srs=EPSG:4326|The output coordinate reference system (the BBOX is expressed in this SRS)|
|format=image/png|The requested format (could have been image/png, image/jpeg, or any other listed in the capabilities document)|

### GetFeatureInfo
The `GetFeatureInfo` request allows a client to issue a “what’s here” request for one or more of the requested layers.

The “here” is specified by giving the coordinate of the pixel that the user clicked, inside the `GetMap` output the user is looking at.

As an example, go to any of the layers' details page and click on a point of the map corresponding to some data. You'll get information about the data on that specific position.

## Creating a Base Map
- From the `Maps` dropdown menu on the `navbar`, click on `Create Map` link
    ![image](https://user-images.githubusercontent.com/1278021/125528667-d7ed8c07-9017-45f1-a48a-09a527a29417.png)

- GeoNode will show an empty map with a default background; from the `burger menu` on top-left corner, click on the `Add Layers` button
    ![image](https://user-images.githubusercontent.com/1278021/125528841-92c3eb97-6f71-4a4d-b63e-5a6b4099bb7e.png)

- The catalog side menu will appear by showing the available layers; start adding the overlays as shwon in the figure below.

  **WARNING**: the order of the overlays is important on a map; you can always change the order by dragging the layer up and down
  
  ![image](https://user-images.githubusercontent.com/1278021/125529081-2d1ce9bf-5125-41e9-91db-b1d7a3725e47.png)

- Change the opacity of the background layer as shown in the figure below. Zoom in and out in order to show/hide the layer details
    ![image](https://user-images.githubusercontent.com/1278021/125529180-a328d26f-48d8-4509-b18a-de64f8c5204e.png)

- Once happy, click on the `top-right burger` menu and select `Save As` from the dropdown list
    ![image](https://user-images.githubusercontent.com/1278021/125529320-9c7c7a3c-1509-43c1-823d-04e7365ef66a.png)

- Provide a name and a description and then click on `Save As` button
    ![image](https://user-images.githubusercontent.com/1278021/125529387-7851d0e3-60c3-4dd4-968a-b89a888bb8fd.png)

- If the machine is particularly slow, the save action might display a `timeout error`; you can ignore it and go to the `maps list`. Select the newly created map in order to enter the `Map details` page
    ![image](https://user-images.githubusercontent.com/1278021/125529519-f4961dbd-0f30-4045-acbc-ed497ffc4798.png)



