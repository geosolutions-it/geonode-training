# Optimizing, publishing and styling GeoTIFF data

## Styling with CSS
The CSS extension module allows to build map styles using a compact, expressive styling language already well known to most web developers: `Cascading Style Sheets`. 

The extension is available on GeoServer only. The standard CSS language has been extended to allow for map filtering and managing all the details of a map production. In this section we'll experience creating a few simple styles with the CSS language.

### Styling raster data

- Go to the `srtm_boulder` layer details page and click on `Editing Tools > Style > Edit`

    ![image](https://user-images.githubusercontent.com/1278021/137093317-8758ef1b-cfe4-42a2-a590-eeb727bc5021.png)

- Add a new `Style`

    ![image](https://user-images.githubusercontent.com/1278021/137093431-28518a78-609e-4963-a3d2-dc4f08d49430.png)

- Select `CSS` and click on the `+` button

    ![image](https://user-images.githubusercontent.com/1278021/137093535-ac7af98e-8b70-4bdd-b311-121c7253d9f5.png)

- Put a name like `CSS Raster` and click on `Save`

    ![image](https://user-images.githubusercontent.com/1278021/137093942-e04caebf-829c-4755-b191-4747603ffd78.png)

- Modify the style you just created

    ![image](https://user-images.githubusercontent.com/1278021/137094038-1f2f7703-77be-481f-a1c7-8e4913799ecc.png)

- From the panel, switch to the advanced editor

    ![image](https://user-images.githubusercontent.com/1278021/137094124-2995c120-9481-442f-a27d-e65c18a8e882.png)

- Insert the following `CSS`, notice the changes directly on the map, and `Save`

    ```css
    * {
       raster-channels: auto;
       raster-color-map:
         color-map-entry(black, 0, 0)
         color-map-entry(green, 1500)

         color-map-entry(yellow, 2000)
         color-map-entry(maroon, 3000)
         color-map-entry(white, 4000);
    }
    ```

    ![image](https://user-images.githubusercontent.com/1278021/137094280-5256f06b-0072-4fdc-a8e5-93c04a28918b.png)

- The `*` at the beginning means that we will apply the rules at any zoom level; let's add a rule to show the raster only above a certain `scale denominator`

    ```css
    [@scale > 75000] {
       raster-channels: auto;
       raster-color-map:
         color-map-entry(black, 0, 0)
         color-map-entry(green, 1500)

         color-map-entry(yellow, 2000)
         color-map-entry(maroon, 3000)
         color-map-entry(white, 4000);
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137096933-31a84c57-31fd-44b8-b6a5-15235f05ea17.png)

- Try to zoom in and out by checking the `scale denominator`; notice the raster appearing and disappearing accordingly

    ![image](https://user-images.githubusercontent.com/1278021/137097185-fa97cc3c-67d6-42fb-8762-cbd71850b21f.png)


## Nice Map with Hillshading

In this section we are going to create a `hillshade` raster computed from the `srtm DEM` of Boulder. We will use the `GDAL` utilities to do that, and in particular the `gdaldem` one, which has several options available.

Once we created the `hillshade`, we will add it to GeoNode, provide a nice style and then merge all together on a nice map.

- Go to the `terminal` and move to the folder `/opt/data/sample_data/pretty_maps/data/boulder/`
- Run the `gdaldem` utility with the following options

    ```shell
    gdaldem hillshade -z 5 -s 111120 srtm_boulder.tiff srtm_boulder_hs.tiff -co tiled=yes
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137097922-491e1c69-ae69-402a-9901-fddb11bb8049.png)

    _The z parameter exaggerates the elevation, the s parameter provides the ratio between the elevation units and the ground units (degrees in this case), -co tiled=yes makes gdaldem generate a TIFF with inner tiling. We’ll investigate this last option better in the following pages._
    
    
- That will create a new `GeoTIFF` called `srtm_boulder_hs.tiff`
- Upload the new layer as user `test_user1`

    ![image](https://user-images.githubusercontent.com/1278021/137100273-ed2e1cbd-2ab4-4aeb-b5cc-47cd8a3d10e3.png)

- Edit the style for this layer too; a new `CSS` named `Hillshade CSS`

    ![image](https://user-images.githubusercontent.com/1278021/137101389-4912ae6a-1af1-44f6-a03d-451fab812522.png)

    ![image](https://user-images.githubusercontent.com/1278021/137101508-24adac41-c435-4b73-a3f7-1821ef1bf157.png)

- Insert the `CSS` below and save

    ```css
    [@scale > 75000] {
      raster-channels: auto;
      raster-color-map:
        color-map-entry(#000000, 0, 0)
        color-map-entry(#999999, 1)
        color-map-entry(#FFFFFF, 256);
      composite: 'multiply';
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137102110-501d834d-bbb2-48fc-a9a5-5db710bbc070.png)

    _Note the “composite” vendor option, used to have the hillshading use a special color blending mode to get a better visual result compared to simple translucency_

- Create a new `Map` and add the two raster layers just updated

    ![image](https://user-images.githubusercontent.com/1278021/137102908-cf8a822c-711a-4fd3-b520-942ee96a833e.png)

- Highlight the `srtm_boulder` and the `modify` option

    ![image](https://user-images.githubusercontent.com/1278021/137103087-f6c7e17f-cdcd-4aa2-8574-87b110683441.png)

- Select the `style` tab, click on the `CSS Raster` style and `Save`

    ![image](https://user-images.githubusercontent.com/1278021/137103288-facb2bac-9356-46bb-b531-ad80893cec83.png)

- Notice the `legend` has changed and so the rendering on the map for that layer

    ![image](https://user-images.githubusercontent.com/1278021/137103430-af6aca79-ad6d-4a09-afea-bf8a3a5addaf.png)

- Similarly go ahead and select the `Hillshade CSS` for the `srtm_boulder_hs` layer

    ![image](https://user-images.githubusercontent.com/1278021/137103640-2d55617f-b364-415b-b596-65dcbc90c514.png)

- Adjust the overlays `order` and `opacity` and notice the map changing accordingly

    ![image](https://user-images.githubusercontent.com/1278021/137103877-ce5d7e20-ee4b-4d6c-a2d2-b263bafea938.png)

- Zoom-in in order to highlight the details and the color effect enhanced by the hillshade

    ![image](https://user-images.githubusercontent.com/1278021/137104006-f7a0c019-c211-4fd1-9550-47275e019d30.png)

- Save the new map when done

    ![image](https://user-images.githubusercontent.com/1278021/137104141-3bd67642-2826-441c-84da-0c624e9544fa.png)


## Adding and Optimizing Raster Image Mosaics

An `Image Mosaic` is composed of a set of `datasets` which are exposed as a `single coverage`. The `ImageMosaic` format allows to **automatically** build and setup a mosaic from a set of `georeferenced datasets`. This section provides better instructions to configure an `Image Mosaic`.

### Publishing an Image Mosaic

In this section we are going to publish on GeoNode a `mosaic` of aerial `orthophotos` through the GeoServer `ImageMosaic` store.

We will need to do some preparation first.

- Open a `terminal` and move to the folder `/opt/data/sample_data/user_data`; change the `write` permissions of the folder named `aerial` in order to allow the user to modify it

    ```shell
    cd /opt/data/sample_data/user_data
    sudo chmod -Rf 777 aerial
    ```

- Move to `aerial` and delete the files `aerial.properties` and `sample_image.dat`

    ```shell
    cd aerial
    rm aerial.properties sample_image.dat
    ```

- Create or edit a file named `indexer.properties` with the following contents

    ```shell
    vim indexer.properties
    ```

    ```ini
    Caching=false
    Schema=*the_geom:Polygon,location:String
    ```

- Create or edit a file named `datastore.properties` with the following contents

    ```shell
    vim datastore.properties
    ```

    ```ini
    SPI=org.geotools.data.postgis.PostgisNGDataStoreFactory
    host=localhost
    port=5432
    database=geonode_data
    schema=public
    user=geonode
    passwd=geonode
    Loose\ bbox=true
    Estimated\ extends=false
    validate\ connections=true
    Connection\ timeout=10
    preparedStatements=true
    create\ database\ params=WITH\ TEMPLATE\=postgis20
    ```

- Go to the GeoServer admin dahsboard; log in as an `admin` and click on `Data > Stores > Add new store`

    ![image](https://user-images.githubusercontent.com/1278021/137112715-7e655f77-9243-4e4b-a9e5-277583a210f5.png)

- Select `ImageMosaic` form the list of the available `Raster Data Sources`

    ![image](https://user-images.githubusercontent.com/1278021/137112890-18769296-902a-429c-86fe-43169b6dc1ba.png)

- Provide the following inputs on the `ImageMosaic` form

    * **Name**: `Aerial`
    * **Title**: `Aerial`
    * **URL***: `file:/opt/data/sample_data/user_data/aerial`

    ![image](https://user-images.githubusercontent.com/1278021/137113263-dd6d600b-7274-4ee9-9dee-522fb74df0d9.png)

- On the next windows, click on `Publish` and `Save` the layer

    ![image](https://user-images.githubusercontent.com/1278021/137113436-c82e10d3-d9de-4b39-a911-1f128e5faa0c.png)

    ![image](https://user-images.githubusercontent.com/1278021/137113518-8ea8a064-06c7-4926-b7b7-e6d29ff4adc3.png)

- Move to the GeoServer `Layer Preview` section and open the `OpenLayers` preview for the `Aeriel` layer

    ![image](https://user-images.githubusercontent.com/1278021/137114837-8e4f780d-39af-4d83-b986-9c2bf4f9e8b8.png)

- We will suddenly notice two issues mainly:

    1. The load, pan and zoom is _slow_
    2. There are some _black_ tiles filling the bounding box

    ![image](https://user-images.githubusercontent.com/1278021/137115090-1d654edb-0882-45f9-992d-edb57fef4567.png)

### Optimizing an Image Mosaic

- Open a `terminal` and move to the folder `/opt/data/sample_data/user_data`; change the `write` permissions of the folder named `optimized` in order to allow the user to modify it

    ```shell
    cd /opt/data/sample_data/user_data
    sudo chmod -Rf 777 optimized
    ```

- Move to `optimized` and delete the files `optimized.properties` and `sample_image.dat`

    ```shell
    cd optimized
    rm optimized.properties sample_image.dat
    ```

- Create or edit a file named `indexer.properties` with the following contents

    ```shell
    vim indexer.properties
    ```

    ```ini
    Caching=false
    Schema=*the_geom:Polygon,location:String
    ```

- Create or edit a file named `datastore.properties` with the following contents

    ```shell
    vim datastore.properties
    ```

    ```ini
    SPI=org.geotools.data.postgis.PostgisNGDataStoreFactory
    host=localhost
    port=5432
    database=geonode_data
    schema=public
    user=geonode
    passwd=geonode
    Loose\ bbox=true
    Estimated\ extends=false
    validate\ connections=true
    Connection\ timeout=10
    preparedStatements=true
    create\ database\ params=WITH\ TEMPLATE\=postgis20
    ```

- Add a new `ImageMosaic Data Source` like we already done before and provide the following parameters

- Provide the following inputs on the `ImageMosaic` form

    * **Name**: `boulder_bg_optimized`
    * **Title**: `boulder_bg_optimized`
    * **URL***: `file:/opt/data/sample_data/user_data/optimized`

    ![image](https://user-images.githubusercontent.com/1278021/137135458-349f8209-4e87-4811-81c7-c1532edcc63f.png)

- On the next windows, click on `Publish`, **change the name and title to** `boulder_bg_optimized` and `Save` the layer

    ![image](https://user-images.githubusercontent.com/1278021/137135716-ec870eae-1499-46d0-9d47-e0e99e0c8c50.png)

- Open the GeoServer `Layer Preview` and zoom/pan around, notice how faster is now the mosaic...

    ![image](https://user-images.githubusercontent.com/1278021/137136033-4620044b-345a-4f6d-bd6d-bcdfde7a8ddf.png)

    _The black tiles are still present._

- Let's customize the mosaic options in order to improve the quality and remove the black tiles.
- Customize the `ImageMosaic` properties. Set the `OutputTransparentColor` to the value `000000` (Which is the `Black` color). Click on `Save` when done.

    ![image](https://user-images.githubusercontent.com/1278021/137136730-24378b4c-8128-4e51-974a-38408e7b9fb5.png)

- Open the GeoServer `Layer Preview`, **clear the browser image cache** and zoom/pan around, notice the black tiles now gone

    ![image](https://user-images.githubusercontent.com/1278021/137136942-5a66ff2e-c101-4a9d-a8b0-1f2ec5958509.png)

## Image Mosaic Options

There is plenty of options that can be tweaked in order to further optimize an `ImageMosaic`.

* **Accurate resolution computation**: If `true`, computes the resolution of the granules in 9 points, the corners of the requested area and the middle points and take the better one. This will provide better results for cases where there is a lot more deformation on a subregion (top/bottom/sides) of the requested bbox with respect to others. Otherwise, compute the resolution using a basic affine scale transform.
* **AllowMultithreading**: If `true`, enables tiles multithreading loading. This allows to perform parallelized loading of the granules that compose the mosaic.
* **BackgroundValues**: Sets the value of the mosaic `background`. Depending on the nature of the mosaic it is wise to set a value for the `no data` area (usually `-9999`). This value is _repeated on all the mosaic bands_.
* **Bands**: a comma separated list of band indexes that allows to expose only a particular subset of bands out of a multiband source. Normally not configured by hand, but driven by `SLD` styling instead.
* **ExcessGranuleRemoval**: An option that should be enabled when using _scattered and deeply overlapping images_. By default the image mosaic will try to mosaic toghether all images in the requested area, even if some are behind and won’t show up in the final image. With excess granule removal the system will use the image footprint to determine which granules actually contribute pixels to the output, and will end up performing the image processing only on those actually contributing. Best used along with footprints and sorting (to control which images actually stay on top). Possible values are `NONE` or `ROI` (_Region Of Interest_).
* **Filter**: Filters granules based on attributes from the input coverage.
* **Footprint Behavior**: Sets the behavior of the regions of a granule that are _outside_ of the granule footprint. Can be `None` (_Ignore the footprint_), `Cut` (_Remove regions outside the footprint from the image. Does not add an alpha channel_), or `Transparent` (_Make regions outside the footprint completely transparent. Will add an alpha channel if one is not already present_). Defaults to `None`.
* **InputTransparentColor**: Sets the transparent color for the granules prior to mosaicking them in order to control the superimposition process between them. When GeoServer composes the granules to satisfy the user request, some of them can overlap some others, therefore, setting this parameter with the opportune color avoids the overlap of no data areas between granules.
* **MaxAllowedTiles**: Sets the maximum number of the tiles that can be load simultaneously for a request. In case of a large mosaic this parameter should be opportunely set to not saturating the server with too many granules loaded at the same time.
* **MergeBehavior**: Merging behaviour for the various granules of the mosaic that GeoServer will produce. This parameter controls whether we want to merge in a single mosaic or stack all the bands into the final mosaic.
* **OVERVIEW_POLICY**: Policy used to select the best matching overview for a given target output resolution. Possible values are `QUALITY` (pick an overview with a higher resolution and downsample), `NEAREST` (pick the one with the closest resolution), `SPEED` (pick the closest one with lower resolution) and `IGNORE` (do not use overviews).
* **OutputTransparentColor**: Sets the transparent color for the created mosaic.
* **SORTING**: Allows to specify the time order of the obtained granules set. Valid values are `DESC` (descending) or `ASC` (ascending). Note that it works just by using `DBMS` as indexes.
* **SUGGESTED_TILE_SIZE**: Controls the tile size of the input granules as well as the tile size of the output mosaic. It consists of two positive integers separated by a comma, like `512,512`.
* **USE_JAI_IMAGEREAD**: If `true`, GeoServer will make use of `JAI ImageRead` operation and its deferred loading mechanism to load granules; if `false`, GeoServer will perform direct `ImageIO` read calls which will result in immediate loading.

## Introduction To Processing With GDAL Utilities



#### [Next Section: Optimizing, publishing and styling Vector data](OPTIMIZE_VECTOR.md)
