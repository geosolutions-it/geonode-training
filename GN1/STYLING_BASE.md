# Styling with GeoNode
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


