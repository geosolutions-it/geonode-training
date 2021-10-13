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


#### [Next Section: Optimizing, publishing and styling Vector data](OPTIMIZE_VECTOR.md)
