# Styling with GeoNode (Basics)
This section introduces the concepts of the **Styled Layer Descriptor** (`SLD`) markup language.

`SLD` is the styling engine used by the GIS backend (i.e. GeoServer). It allows us to create beautiful and informative portryals out of raw geospatial data.

In this section you will also use the `CSS` [extension module](http://docs.geoserver.org/latest/en/user/styling/css/index.html) that allows you to build map styles using a compact, expressive styling language already well known to most web developers: **Cascading Style Sheets**.
The standard `CSS` language has been extended to allow for map filtering and managing all the details of a map production.

The examples will have styles in `SLD` and `CSS` language.

**WARNING**: `CSS` styling is supported by GeoServer only, which is the default GIS backend provided with GeoNode. In the [official GeoServer documentation](http://docs.geoserver.org/latest/en/user/styling/index.html#styling) you can find useful references as the [SLD Cookbook](http://docs.geoserver.org/latest/en/user/styling/sld/cookbook/index.html) and the [CSS Cookbook](http://docs.geoserver.org/latest/en/user/styling/css/cookbook/index.html).

## Uploading and Replacing the Layer Default Style
This procedure allows us to quickly update andh replace the current layer default style with a new one from the hard disk.

**WARNING**: Currently this procedure works only with `SLD` files

- Before using the style, we will need to sligthly modify it in order to match the correct field names of the `Mainrd Layer`

- Go to the `Mainrd` layer detail page; click on the `Attributes` tab panel just below the map preview
    * Take note of the `LABEL_NAME` attribute and remember that `SLD` is *case sensitive*

    ![image](https://user-images.githubusercontent.com/1278021/125473334-3e4f3c17-5c12-43bf-b061-3d725a1f4ebc.png)

- Click on the system `File Browser`, navigate to `/opt/data/sample_data/user_data`
    * `RIGHT-CLICK` with the mouse on the file named `foss4g_mainrd.sld`
    * Select and click `Open with Other Application`
 
    ![image](https://user-images.githubusercontent.com/1278021/125472443-16727492-2e25-4d2c-834d-1cf0cce02c3f.png)

- Click on `View all applications` button and then select `Text Editor` and `Select`
    ![image](https://user-images.githubusercontent.com/1278021/125473890-47a88488-89c1-4704-8061-fd8432340715.png)

- Change the `label_name` to uppercase `LABEL_NAME`, click `Save` and `close`
    ![image](https://user-images.githubusercontent.com/1278021/125474327-8f68f1e6-6ffe-4dd0-a9b8-aedb52b779ef.png)

- Go to the `Mainrd` layer detail page; click on the `Editing Tools` button and the on `Style > Upload`
    ![image](https://user-images.githubusercontent.com/1278021/125468623-3e2c6bee-0d96-4075-adcc-b8fdfb632841.png)

- From the `Style Upload` page, click on the `Choose files` button and navigate to `/opt/data/sample_data/user_data`, select the file `foss4g_mainrd.sld` and then click on `Open`
    ![image](https://user-images.githubusercontent.com/1278021/125468962-9bfd3df5-e734-4947-aba5-d5f947e7819a.png)

- Click on `Upload file` and, once finished, on `Return to Layer`
    ![image](https://user-images.githubusercontent.com/1278021/125474719-af6321bc-039c-4be7-9d8d-66f6ba2bcee8.png)

  **WARNING**: If the page does not refresh correctly, click together `CTRL-SHIFT-CANC` and clear the browser image cache from the last hour
  
     ![image](https://user-images.githubusercontent.com/1278021/125475210-f204713a-cac0-478f-9b0b-c178cff9a8dd.png)

- Go back to the layer detail page, refresh the page; notice how the `Legend` has changed and also the styling of the layer. Trying to zoom in, you will notice also some labels appering.
    ![image](https://user-images.githubusercontent.com/1278021/125475684-be2c1907-e031-49ce-b164-cf32b49e12a9.png)

- Go ahead and update the `Thumbnail` by clicking on the Editing Tools menu and then on the `Thumbnail > Set` link
    ![image](https://user-images.githubusercontent.com/1278021/125491670-ae82feff-4753-4587-ae95-37bc8a5d829a.png)

- Once the thumbnail has been saved
    ![image](https://user-images.githubusercontent.com/1278021/125491828-63ef4644-dcfd-45de-9fab-fddbcf79b158.png)

- Go back to the layers list and verify is has been updated
    ![image](https://user-images.githubusercontent.com/1278021/125491890-6dfe8ab4-bb04-4046-a670-34a59a0261fb.png)

## Adding more Base Layers and Styles

### Boulder City Limits
- From the location `/opt/data/sample_data/pretty_maps/data/boulder` upload the layer `BoulderCityLimits`
    ![image](https://user-images.githubusercontent.com/1278021/125492291-7a273f3d-ea37-4362-9cf8-45a252c1ad0b.png)

- From the location `/opt/data/sample_data/pretty_maps/styles` upload the style `foss4g_citylimits.sld`
    ![image](https://user-images.githubusercontent.com/1278021/125492744-294439f0-6d4a-4a2d-a5ed-98b4df9d477f.png)

- The layer and legend should appear as shown here
    ![image](https://user-images.githubusercontent.com/1278021/125492951-1ee1ba46-95b4-44ca-a2fc-04fb7bac8637.png)

- Set the thumbnail as we have done previously

### Buildings
- From the location `/opt/data/sample_data/pretty_maps/data/boulder` upload the layer `Buildings050714`
  **WARNING**: pay attention to select the files shown in figure, do not include the `.sld` and `.xml` ones
  
    ![image](https://user-images.githubusercontent.com/1278021/125493133-a5ec47ba-d37d-40be-b271-376bc457222d.png)

- From the location `/opt/data/sample_data/pretty_maps/styles` upload the style `foss4g_buildings.sld`
- Notice that the Legend presents 3 different symbols, notice also that no feature will be visibile unless you zoom very close to the buildings
    ![image](https://user-images.githubusercontent.com/1278021/125494060-c8bebb58-15fc-4e53-9187-9e5da3deb17b.png)

- By taking a closer look at the style source code, we can notice that:
    * We have a set of `Rule` elements each one constrained by some `ScaleDenominator` limits.
    * The `Shadow` rule, defines also a `Displacement` making the polygons translate on the `X-Y` layer
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
      <sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" xmlns:sld="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml" version="1.0.0">
        <sld:UserLayer>
          <sld:LayerFeatureConstraints>
            <sld:FeatureTypeConstraint/>
          </sld:LayerFeatureConstraints>
          <sld:UserStyle>
            <sld:Name>Buildings050714</sld:Name>
            <sld:Title/>
            <sld:IsDefault>1</sld:IsDefault>
            <sld:FeatureTypeStyle>
              <sld:Rule>
                <sld:Name>Shadow</sld:Name>
                <sld:MinScaleDenominator>5000.0</sld:MinScaleDenominator>
                <sld:MaxScaleDenominator>15000.0</sld:MaxScaleDenominator>
                <sld:PolygonSymbolizer>
                  <sld:Geometry>
                    <ogc:Function name="offset">
                      <ogc:PropertyName>the_geom</ogc:PropertyName>
                      <ogc:Literal>6.0</ogc:Literal>
                      <ogc:Literal>-6.0</ogc:Literal>
                    </ogc:Function>
                  </sld:Geometry>
                  <sld:Fill/>
                </sld:PolygonSymbolizer>
              </sld:Rule>
            </sld:FeatureTypeStyle>
            <sld:FeatureTypeStyle>
              <sld:Rule>
                <sld:Name>Label</sld:Name>
                <sld:MaxScaleDenominator>10000.0</sld:MaxScaleDenominator>
                <sld:PolygonSymbolizer>
                  <sld:Fill>
                    <sld:CssParameter name="fill">#FF0000</sld:CssParameter>
                  </sld:Fill>
                </sld:PolygonSymbolizer>
                <sld:TextSymbolizer>
                  <sld:Label>
                    <ogc:PropertyName>LABEL_NAME</ogc:PropertyName>
                  </sld:Label>
                  <sld:Font>
                    <sld:CssParameter name="font-family">Arial</sld:CssParameter>
                    <sld:CssParameter name="font-size">12.0</sld:CssParameter>
                    <sld:CssParameter name="font-style">normal</sld:CssParameter>
                    <sld:CssParameter name="font-weight">normal</sld:CssParameter>
                  </sld:Font>
                  <sld:LabelPlacement>
                    <sld:PointPlacement>
                      <sld:AnchorPoint>
                        <sld:AnchorPointX>
                          <ogc:Literal>0.5</ogc:Literal>
                        </sld:AnchorPointX>
                        <sld:AnchorPointY>
                          <ogc:Literal>0.5</ogc:Literal>
                        </sld:AnchorPointY>
                      </sld:AnchorPoint>
                      <sld:Displacement>
                        <sld:DisplacementX>
                          <ogc:Literal>0.0</ogc:Literal>
                        </sld:DisplacementX>
                        <sld:DisplacementY>
                          <ogc:Literal>0.0</ogc:Literal>
                        </sld:DisplacementY>
                      </sld:Displacement>
                      <sld:Rotation>
                        <ogc:Literal>0.0</ogc:Literal>
                      </sld:Rotation>
                    </sld:PointPlacement>
                  </sld:LabelPlacement>
                  <sld:Halo>
                    <sld:Radius>
                      <ogc:Literal>2</ogc:Literal>
                    </sld:Radius>
                    <sld:Fill>
                      <sld:CssParameter name="fill">#FFFFFF</sld:CssParameter>
                    </sld:Fill>
                  </sld:Halo>
                  <sld:Fill>
                    <sld:CssParameter name="fill">#000000</sld:CssParameter>
                  </sld:Fill>
                  <sld:VendorOption name="autoWrap">100</sld:VendorOption>
                  <sld:VendorOption name="maxDisplacement">50</sld:VendorOption>
                </sld:TextSymbolizer>
              </sld:Rule>
              <sld:Rule>
                <sld:Name>fill</sld:Name>
                <sld:MaxScaleDenominator>25000.0</sld:MaxScaleDenominator>
                <sld:PolygonSymbolizer>
                  <sld:Fill>
                    <sld:CssParameter name="fill">#B3B3B3</sld:CssParameter>
                  </sld:Fill>
                </sld:PolygonSymbolizer>
              </sld:Rule>
            </sld:FeatureTypeStyle>
          </sld:UserStyle>
        </sld:UserLayer>
      </sld:StyledLayerDescriptor>
     ```

- Set the thumbnail as we have done previously

### Parcels
- From the location `/opt/data/sample_data/pretty_maps/data/boulder` upload the layer `Parcels`
    ![image](https://user-images.githubusercontent.com/1278021/125496679-5de686c3-68e7-486b-908d-2ee4454eebf6.png)

- From the location `/opt/data/sample_data/pretty_maps/styles` upload the style `foss4g_parcels.sld`; the layer appear only at very high levels of zoom
    ![image](https://user-images.githubusercontent.com/1278021/125497076-f23a3f82-adaa-43b3-a748-3f3c84f46698.png)

- Set the thumbnail as we have done previously

### Pointlm
- From the location `/opt/data/sample_data/pretty_maps/data/boulder` upload the layer `pointlm`
- From the location `/opt/data/sample_data/pretty_maps/styles` upload the style `foss4g_point_landmark_ds_ns.sld`
- At a first glance, the layer appear as a set of grey suqare points; let's take a closer look at the `SLD` definition

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
      <sld:StyledLayerDescriptor
      xmlns="http://www.opengis.net/sld"
      xmlns:sld="http://www.opengis.net/sld"
      xmlns:ogc="http://www.opengis.net/ogc"
      xmlns:gml="http://www.opengis.net/gml"
      xmlns:xlink="http://www.w3.org/1999/xlink" version="1.0.0">

        <sld:UserLayer>
          <sld:LayerFeatureConstraints>
            <sld:FeatureTypeConstraint/>
          </sld:LayerFeatureConstraints>
          <sld:UserStyle>
            <sld:Name>tl 2010 08013 pointlm</sld:Name>
            <sld:Title/>
            <sld:FeatureTypeStyle>
              <sld:Rule>
                <sld:Name>landmarks</sld:Name>
                <ogc:Filter>
                    <ogc:PropertyIsGreaterThan>
                      <ogc:Function name="strLength">
                        <ogc:PropertyName>IMAGE</ogc:PropertyName>
                      </ogc:Function>
                      <ogc:Literal>0</ogc:Literal>
                    </ogc:PropertyIsGreaterThan>
                </ogc:Filter>
                <sld:PointSymbolizer>
                  <sld:Graphic>
                    <sld:ExternalGraphic>
                      <sld:OnlineResource xlink:type="simple" xlink:href="./img/landmarks/${IMAGE}" />
                      <sld:Format>image/png</sld:Format>
                    </sld:ExternalGraphic>
                  </sld:Graphic>
                  <VendorOption name="labelObstacle">true</VendorOption>
                </sld:PointSymbolizer>
                <sld:TextSymbolizer>
                  <sld:Label>
                    <ogc:PropertyName>FULLNAME</ogc:PropertyName>
                  </sld:Label>
                  <sld:Font>
                    <sld:CssParameter name="font-family">Arial</sld:CssParameter>
                    <sld:CssParameter name="font-size">12.0</sld:CssParameter>
                    <sld:CssParameter name="font-style">normal</sld:CssParameter>
                    <sld:CssParameter name="font-weight">normal</sld:CssParameter>
                  </sld:Font>
                  <sld:LabelPlacement>
                    <sld:PointPlacement>
                      <sld:AnchorPoint>
                        <sld:AnchorPointX>
                          <ogc:Literal>0.5</ogc:Literal>
                        </sld:AnchorPointX>
                        <sld:AnchorPointY>
                          <ogc:Literal>1.0</ogc:Literal>
                        </sld:AnchorPointY>
                      </sld:AnchorPoint>
                      <sld:Displacement>
                        <sld:DisplacementX>
                          <ogc:Literal>0.0</ogc:Literal>
                        </sld:DisplacementX>
                        <sld:DisplacementY>
                          <ogc:Literal>-14.0</ogc:Literal>
                        </sld:DisplacementY>
                      </sld:Displacement>
                      <sld:Rotation>
                        <ogc:Literal>0.0</ogc:Literal>
                      </sld:Rotation>
                    </sld:PointPlacement>
                  </sld:LabelPlacement>
                  <sld:Halo>
                    <sld:Radius>
                      <ogc:Literal>1.5</ogc:Literal>
                    </sld:Radius>
                    <sld:Fill>
                      <sld:CssParameter name="fill">#FFFFFF</sld:CssParameter>
                    </sld:Fill>
                  </sld:Halo>
                  <sld:Fill>
                    <sld:CssParameter name="fill">#000033</sld:CssParameter>
                  </sld:Fill>
                  <sld:Priority>200000</sld:Priority>
                  <sld:VendorOption name="autoWrap">100</sld:VendorOption>
                </sld:TextSymbolizer>
              </sld:Rule>
            </sld:FeatureTypeStyle>
          </sld:UserStyle>
        </sld:UserLayer>
      </sld:StyledLayerDescriptor>
   ```

   We can notice the notation `xlink:href="./img/landmarks/${IMAGE}"`; this is a GeoServer extension allowing us to read some values from the source dataset attributes and use them to dynamically render the `SLD`.

  In that specific case, GeoServer looks for some file names accordingly to the `${IMAGE}` attribute value into a relative folder named `./img/landmarks/`.
  
  In order to correctly render this style, we will need to copy this folder containing the images correctly named, into the GeoServer `styles` folder.
  
  We will have also to give to that folder the correct permissions allowing GeoServer to be able to access it.
  
- The first step is to copy the `img` folder into the correct GeoServer `styles` one
   * Open a `system file browser` window and go to `/opt/data/sample_data/pretty_maps/styles`
   * `RIGHT-CLICK` on the `img` folder
   * Select `Copy` from the side menu

   ![image](https://user-images.githubusercontent.com/1278021/125500351-4e9bc2dc-3365-4e41-aa24-359d8b8c7e32.png)

- From the `system file browser` window go to `/opt/data/geoserver_data/workspaces/geonode/`; it will probably ask for `ROOT` permissions. Just insert the password `geonode` everytime it asks for
    ![image](https://user-images.githubusercontent.com/1278021/125500660-64aa4008-7351-4fca-9fe7-e7ca2f8db221.png)

- Go to the styles folder inside the geonode workspace and hit `CTRL+V`; this will copy the `img` folder into the new location
     ![image](https://user-images.githubusercontent.com/1278021/125500772-e3cda6da-2880-4949-b6df-51e1eaa8e890.png)

- `RIGHT-CLICK` on the newly created `img` folder and click on the `Properties` link of the context menu
     ![image](https://user-images.githubusercontent.com/1278021/125511826-992480b8-83a4-4669-8c92-3566d447fd05.png)

- Assign the permissions to the `others group` like shown in the figure below
     ![image](https://user-images.githubusercontent.com/1278021/125511930-e603e010-3ccd-4e3c-98a6-734b3ca3eed6.png)

- Refresh the layer page, you should be able to see the icons appearing on the map preview
     ![image](https://user-images.githubusercontent.com/1278021/125512068-36e44fdd-f8d3-4d1d-b8da-db5bb5ec2d02.png)

- Set the thumbnail as we have done previously

### Remaining Boulders Layers
- From the location `/opt/data/sample_data/pretty_maps/data/boulder` upload the layers:
   * `srtm_boulder`
   * `Streets`
   * `Trails`
   * `Wetlands_regulatory_area`
   
   ![image](https://user-images.githubusercontent.com/1278021/125514558-d53f817a-8c94-4d90-a7a8-2f37f6206464.png)

- From the location `/opt/data/sample_data/pretty_maps/styles` upload the styles:
   * `foss4g_dem2.sld` to `srtm_boulder`
   * `foss4g_streets.sld` to `Streets`
   * `foss4g_trails.sld` to `Trails`
   * `foss4g_wetlands.sld` to `Wetlands_regulatory_area`

#### [Next Section: Pretty Maps with GeoNode](MAPPING_GEONODE.md)
