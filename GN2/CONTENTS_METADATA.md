# Improving Contents Metadata
Correctly setting the datsets' metadata is very important in GeoNode.

That not only allow the owners to better describe the contents and quality of the dataset, other than eventual restricions or license contraints on its usage, but also the registered member to search for it on the catalog.

## Editing `Mainrd` Metadata

- As an `admin`, or as a user with `editing metadata` permissions on the dataset, move to the `Mainrd` layer details

    ```
    http://localhost/layers/geonode_data:geonode:Mainrd
    ```

- Click on `Editing Tools > Metadata > Wizard`

    ![image](https://user-images.githubusercontent.com/1278021/136559752-7edd7193-726a-4efa-86a8-f98733410731.png)

- On the next form, fill the metadata fields as follows

    - **Title**: `Boulder Main Roads`
    - **Abstract**: `The dataset shows the main roads of the Boulde city, in Colorado.`
    - **Category**: `Transportation`
    - **Free-text Keywords**: `boulder, roads, transportation,`
    - **License**: `Open Data Commons Open Database License / OSM`
    - **Regions**: `United Stated of America [North America], Americas, North America,`
    - **Temporal Extent**: `2021-08-01 to 2021-09-30`

- Click on `Update` and wait for the page to be reloaded; the `info` panel will show the updated information.

    ![image](https://user-images.githubusercontent.com/1278021/136560938-02c8b965-66ce-44cf-b8d9-f30c999ace33.png)


#### [Next Section: Finding Contents](ADV_SEARCH.md)
