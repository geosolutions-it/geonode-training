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

- Let's prepare the datset first; we will restore some DB tables and create the layer on GeoServer
- Open a terminal window and execute the following commands

    ```shell
    cd /opt/data/sample_data/user_data/storm_track_sql
    createdb -U postgres -O geonode storm_track_sql
    psql -U postgres storm_track_sql -c "CREATE EXTENSION postgis;"
    psql -U postgres storm_track_sql < storm_track_sql.db
    psql -U postgres storm_track_sql -c "GRANT SELECT ON TABLE storm_obs TO geonode;"
    ```

    _That will create a new DB and restore a table of the historical storms from an existing dump._

- We need to create the layer on GeoServer first; as an `admin` move to the GeoServer admin gui, click on `Data > Stores` and then `Postgis Data Store`

    ![image](https://user-images.githubusercontent.com/1278021/137323137-126eb0b1-a985-483d-a01d-f35cfdb1f9fd.png)

- Provide the connection parameters to the new DB

    * **Name**: `storm_track`
    * **Description**: `storm_track`
    * **Database**: `storm_track_sql`
    * **Username**: `geonode`
    * **Password**: `geonode`

    ![image](https://user-images.githubusercontent.com/1278021/137323459-03c47e54-d450-43dc-b37a-4867e1535e2f.png)

- Click on `Publish` on the next window

    ![image](https://user-images.githubusercontent.com/1278021/137323771-f7329f48-736e-4e96-ab2c-4f109c54cfda.png)

- Compute the `Data Bounds`

    ![image](https://user-images.githubusercontent.com/1278021/137323865-a3da7ca2-a8ed-40b1-acfb-6b0f0fa44ce8.png)

- Click on the `Dimensions` tab and enable the `Time Dimension` as shown here below

    ![image](https://user-images.githubusercontent.com/1278021/137324106-fd08bd3b-6b04-4ae7-bba6-4734e2c961fc.png)

- The layer is ready and published on GeoServer, we need to import it on GeoNode now
- Move to the terminal window, enable the `geonode` virtual environment and move to the folder `/opt/geonode`; execute the `updatelayers` management command as follows

    ```shell
    workon geonode
    cd /opt/geonode
    ./manage_local.sh updatelayers --skip-geonode-registered -u test_user1 -w geonode -f storm_obs
    ```

- The new layer will be created on GeoNode, it will show only a single point without the `timeslider`

    ![image](https://user-images.githubusercontent.com/1278021/137324678-1b6e0b75-0fd0-4f2f-835c-51c4ed4d8b83.png)

- Edit the metadata and enable the `Has Time` checkbox

    ![image](https://user-images.githubusercontent.com/1278021/137324793-0c38d064-a912-493b-859c-51f8de467d92.png)

- Save it and go back to the details page

    ![image](https://user-images.githubusercontent.com/1278021/137324899-8d147c61-8bfa-4a54-9fb8-55b724a7fb7d.png)

- Upload the `storm_obs.sld` style file from the folder `/opt/data/sample_data/pretty_maps/styles`

    ![image](https://user-images.githubusercontent.com/1278021/137325083-6e10bb10-a530-4bca-af4a-a3d24c5ed7f0.png)

    ![image](https://user-images.githubusercontent.com/1278021/137325129-6b84ee4e-6cc7-4b37-becc-5a6d361a85e7.png)

- Go to the `Layer View`, expand the `timeslider` and try to move through the valid temporal instant and intervals

    ![image](https://user-images.githubusercontent.com/1278021/137325431-a4f74f12-bb53-4e48-b90c-9297a6c68e60.png)

    ![image](https://user-images.githubusercontent.com/1278021/137325479-300f0c23-3263-4bc4-ad46-4604e400ef51.png)

    ![image](https://user-images.githubusercontent.com/1278021/137325517-5257f7b6-537c-4441-bb7b-58c8334a9de6.png)


#### [Next Section: Advanced maps publishing and management](../ADV_MAPS_PUB.md)
