# Optimizing Maps, tips and tricks

- As `test_user1` edit the `Boulder` map

    ![image](https://user-images.githubusercontent.com/1278021/137326960-6b5054a7-40db-45e8-b97d-c414df1ebdb7.png)

- Expand the layers list and highlight the `boulder_bg_optimized` one; click on the `property tool`

    ![image](https://user-images.githubusercontent.com/1278021/137327140-cdfe9e62-e4fe-40e2-8d95-ee7af9186156.png)

- Switch to the `view` options tab; you will notice few settings that can be tweaked on each layer in order to improve the map quality or speed

    * **Format**: Allows us to set the output media type from the ones supported by the spatial service.
    * **Tile Size**: Usually this value is set to `256x256` pixels; GeoNode sets them to `512x512`. This value allows us to get tiled requests to the `Tile Service`. A higher size allows GeoNode to reduce the number of requests per layer.
    * **Visibility Limits**: Have the same effects of the `Scale Denominator` rules on the styles. The two options are always applied independently.
    * **Transparent**: When `enabled`, allows the server to render the `alpha` transparent channel where no data is present.
    * **Use cache options**: Whether or not use the tile service (`WMTS`) or not (`WMS`).
    * **Single Tile**: When `enabled` GeoNode won't create tiled requests for this layer, but a single one of the dimension of the current viewport. In some cases this might speed up the rendering by drastically reducing the number of requests. Be aware that `Single Tile` will never use the server cache.

    ![image](https://user-images.githubusercontent.com/1278021/137327337-1902192f-7730-4d91-9afe-14a3b8a7d122.png)

- For this layer, let's change the output format to `jpeg+png8` and set the `Min Scale` to some value in order to hide the layer at highest zoom levels.

    ![image](https://user-images.githubusercontent.com/1278021/137329029-16f8edd3-b6b3-4ccc-aa05-6c3631b3c014.png)
    
    _The JPEG+PNG8 output format will allow GeoServer to use JPEG compressions on images and PNG8 on transparent tiles. This will reduce the size of the images speeding up the layer, but it will also reduce its quality. That option is typically useful on raster imagery only._

- Let's edit the options for the `Streets` layer

    ![image](https://user-images.githubusercontent.com/1278021/137329527-cf546650-967c-4362-b36f-fb9d79ba6cf2.png)

- Change the output format to `PNG8`; on vectorial layers with few colors, like in this case, this will reduce a lot the size of the images, resulting in an overall speed up of the layer.

    ![image](https://user-images.githubusercontent.com/1278021/137329729-86b149dd-e9c5-4489-ac91-6555f87c260b.png)

- Open the `Network` tab of the browser and inspect the `ows` requests; notice the different output format of the requests for the `geonode:Streets` layer. Do some tries by chanigng the output format and notice the difference between the sizes and timings of the outcomes.

    ![image](https://user-images.githubusercontent.com/1278021/137329987-13b5b9e5-1fe9-4f22-b191-bfecf1670532.png)
    
    ![image](https://user-images.githubusercontent.com/1278021/137330313-514ff6a3-3b05-4aa3-a92b-24f4bf1b8ed2.png)


- Also, by expanding the `Headers` section, you will notice how the `geowebcache` responses match with a `Cache HIT`, meaning that we are correctly asking for cached tiles to the `WMTS` service

    ![image](https://user-images.githubusercontent.com/1278021/137330250-d8b3686c-1833-41b8-9d80-e92583225380.png)

- From a map it is possible also to select which one from the available styles (legends) to use on each layer; as an instance let's use the `CSS` style for the `Trials` layer

    ![image](https://user-images.githubusercontent.com/1278021/137330627-6c616193-0f58-40ba-a890-933b28407342.png)

    ![image](https://user-images.githubusercontent.com/1278021/137330700-488a6562-371b-435c-8a12-b7477fc7ed12.png)

- When changing the settings for some layers on a map with many overlays, it is a good practice to temporarly deactivate the others in order to speed up the work, by avoiding to load all of them everytime

    ![image](https://user-images.githubusercontent.com/1278021/137331030-92a56f68-c7b9-4be2-b2e4-dcb92152e2b9.png)

- Let's edit the options for the `Mainrd` layer; click on the `Filter` option and check the `Area of Interest` options

    ![image](https://user-images.githubusercontent.com/1278021/137331821-ff083334-36b7-43b5-8b50-088eb57f1390.png)

- Change the `Filter Type` to `Polygon` and the `Geometry Operation` to `Contains`; draw a `polygon` on the map and click `Apply`

    ![image](https://user-images.githubusercontent.com/1278021/137332133-c9525ba7-b036-4ca2-b889-75d9ed7e3243.png)

- Try modifying the `Geometry Operation` to `Intersects` and see the differences

    ![image](https://user-images.githubusercontent.com/1278021/137332485-783d1a44-1980-4e41-bb7d-e8a9e9e41aa5.png)

- Let's try some more advanced filtering options; as an instance it is possible to filter the current layer by using the geometries from some other layer on the map. Go ahead and slect the `Streets` layer from the `Target Layer` select box; change the `Operation` to `Intersects`. Add an attribute filter, as an instance `LABEL_NAME` to `Colorado Ave`. By applying the filter you'll notice the `Mainrd` showing only the features matching the intersecting geometries from `Streets`

    ![image](https://user-images.githubusercontent.com/1278021/137333406-c0505bea-fe12-43d1-a3e8-f5f5cfa4f07f.png)

- Save the filter and go back to the map; notice that a small `filter icon` appears now near the layer name and the map shows only the filtered features

    ![image](https://user-images.githubusercontent.com/1278021/137334205-055092b9-fff7-4241-b360-7e9ae7d534b4.png)

- You can optionally persist the changes on the map.

## Attributes Media Types Rendering on GetFeatureInfo

The `GetFeatureInfo` is a special operation on the `OWS` protocol allwing us to query a dataset on a specific position in order to get back the values stored on the backend.

In the case of a `VECTORIAL` layer, the outcome of the `GetFeatureInfo` is basically a set of `records`, i.e. `key-value` pairs where the `keys` are the `attibutes` from the `schema` below.

In the simplest use case the outcome of the `GetFeatureInfo` is a plain text reporting the list of `attributes` along with the `values` on that specific `location`. As you can imagine, the values can be almost anything, even references to external `links` or `media contents`, like `images`, `videos` or `audios`.

GeoNode is also able to apply, eventually, an `HTML template` to the `GetFeatureInfo` outcome. That means that GeoNode can render the output as an `HTML` content.

From the layer metadata there's the possibility to `edit` such `template`. In particular there are two different ways to enable it:

1. Basic; GeoNode will simply present the list of the available `attributes` allowing the user to change the order, visibility and `media-type` on each one.
2. Advanced; GeoNode will present a `rich-text` editor panel allowing the user to define its own custom `HTML template`.

In this section we will see how the `basic` one works by defining and editing a new `empty layer`.

- Let's first create a new `empty layer` with the following attributes:

    * `geometry type`: `Polygon`
    * `image`: `String`
    * `video`: `String`
    * `audio`: `String`
    * `href`: `String`

    ![image](https://user-images.githubusercontent.com/1278021/137353728-21668fc7-f051-4702-9598-c9338aa19021.png)

- Edit the layer data and add a new `polygon`

    ![image](https://user-images.githubusercontent.com/1278021/137355503-7944b02e-b362-4cb6-91ba-05b0c50d866f.png)



#### [Next Section: Charts and Widgets](MAPS_CHARTS_WIDGETS.md)
