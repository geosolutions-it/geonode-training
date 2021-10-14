# Publishing Vector Time Series

## Publish Temporal Shapefile through GeoNode

- From the folder `/opt/data/sample_data/gisdata/data/good/time` upload the file `boxes_with_date`

    ![image](https://user-images.githubusercontent.com/1278021/137309538-1c707dbb-6869-43dc-a35c-edb129d79aaa.png)

- The upload starts, but it will stop at `50%` asking to the user for further inputs

    ![image](https://user-images.githubusercontent.com/1278021/137309648-e7712cbd-8f76-4d0a-8178-565b462a083c.png)

- Click on the `play` button, you will be redirected to a summary page with the list of the available temporal dimensions

    ![image](https://user-images.githubusercontent.com/1278021/137309741-6cf901fa-5501-41e5-a49b-fa1c6a2ded0a.png)

- Enable the time check, select the `date field` and click on `next`; it will take some time to finalize the upload. When finished you will be redirected to the details page

    ![image](https://user-images.githubusercontent.com/1278021/137309923-1b41c3d8-d5af-4ae1-8982-c3dee552ce47.png)

- Click on `Editing Tools > Metadata > Wizard` and switch to the `Settings` tab; you'll notice that the `Has Time` checkbox has been enabled

    ![image](https://user-images.githubusercontent.com/1278021/137309953-46d36975-9b66-4451-bacc-abf0cf2e9e99.png)
    
    _When che Has Time checkbox it enabled, GeoNode asks to the OWS Service for the time dimension values_

- Notice the `time slider` now has been enabled on the map.


## Publish Temporal Dataset through GeoServer



#### [Next Section: Advanced maps publishing and management](../ADV_MAPS_PUB.md)
