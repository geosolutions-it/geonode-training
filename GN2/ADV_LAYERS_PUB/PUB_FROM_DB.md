# Publishing data from a DB table

In this section we are going to publish on GeoNode a layer which is already present on a DB table.

In particular those are the steps that will allow us to convert the `Test Layer`, the `empty layer` created in the previous sections, to a `Mercator Projected` one and publish it again on GeoNode:

1. Download the `Test Layer` as an `ESRI Shapefile`
2. Use the `GDAL ogr2ogr` utitlities to reproject it from `World Geodetic System 1984 EPSG:4326`, `LatLon` coordinate reference system used in `GPS`,  to `Spherical Mercator EPSG:3857`, `meters` projected coordinate system used for rendering maps in Google Maps, OpenStreetMap, and tiled layers in general.
3. Use the `GDAL ogr2ogr` utitlities to import the file into a local Data Base.
4. Publish the layer on GeoServer.
5. Publish the layer on GeoNode.
6. Try to edit it again from an external client (QGIS).

## Download the Layer as an `ESRI Shapefile`

- Go to the `Test Layer` details page, move to `Download Layer > Data > Zipped Shapefile`

    ![image](https://user-images.githubusercontent.com/1278021/136972151-c8585cd0-eb9e-46de-8549-002eb95cb190.png)

- Store it on a local folder, e.g. `Downloads` (this is the default target folder used by the browser)

## Project to `EPSG:3857`

- Open a terminal windows and move to the `Downloads` folder

    ![image](https://user-images.githubusercontent.com/1278021/136972613-b3f08d54-80e9-4dc8-a5d4-74835cee075a.png)

    ```shell
    cd
    cd Downloads
    ```

- Create a folder named `test_layer` and unzip the contents of the downloaded archive inside it

    ```shell
    mkdir test_layer
    cd test_layer/
    unzip ../test_layer.zip
    ```

- Use the `GDAL ogr2ogr` utitlities to reproject it from `World Geodetic System 1984 EPSG:4326`, `LatLon` coordinate reference system used in `GPS`,  to `Spherical Mercator EPSG:3857`, `meters` projected coordinate system used for rendering maps in Google Maps, OpenStreetMap, and tiled layers in general

    ```shell
    # ogr2ogr <target srs> <source srs> <format> <target filename> <source filename>
    ogr2ogr -t_srs "EPSG:3857" -s_srs "EPSG:4326" -f 'ESRI Shapefile' test_layer_3857.shp test_layer.shp
    ```

- A new `Shapefile` will be created inside the same folder

    ```shell
    geonode-vm-321@geonodevm-3:~/Downloads/test_layer$ ll
    total 48
    drwxrwxr-x 2 geonode-vm-321 geonode-vm-321 4096 Oct 12 15:17 ./
    drwxr-xr-x 3 geonode-vm-321 geonode-vm-321 4096 Oct 12 15:14 ../
    -rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  532 Oct 12 15:17 test_layer_3857.dbf
    -rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  425 Oct 12 15:17 test_layer_3857.prj    <-- the SRS definition file
    -rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  300 Oct 12 15:17 test_layer_3857.shp    <-- the new projected file
    -rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  108 Oct 12 15:17 test_layer_3857.shx
    -rw-rw-r-- 1 geonode-vm-321 geonode-vm-321    5 Oct 12 14:10 test_layer.cst
    -rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  531 Oct 12 14:10 test_layer.dbf
    -rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  335 Oct 12 14:10 test_layer.prj
    -rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  300 Oct 12 14:10 test_layer.shp
    -rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  108 Oct 12 14:10 test_layer.shx
    -rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  219 Oct 12 14:10 wfsrequest.txt
    ```

## Publish as a DB Table

- Use the `GDAL ogr2ogr` utitlities to import the file into a local Data Base

    ```shell
    ogr2ogr -f "PostgreSQL" PG:"host=localhost user=geonode password=geonode dbname=geonode_data" -overwrite test_layer_3857.shp
    ```

- Login into GeoServer as an `admin`

    ![image](https://user-images.githubusercontent.com/1278021/136974466-e6161a87-f1bb-429b-8d34-373cacd6f0bd.png)

- Move to `Data > Layers` and click on `Add new Layer`

    ![image](https://user-images.githubusercontent.com/1278021/136974706-072b46a1-8627-47c4-9f1e-a18178f88c10.png)

- Select `geonode:geonode_data`, the data store connected to the `geonode_data` DB, created and managed by GeoNode

    ![image](https://user-images.githubusercontent.com/1278021/136974940-344573f4-73c3-4848-97f1-b8263b00373b.png)

- From the list of available tables, select `test_layer_3857` and click on `Publish`

    ![image](https://user-images.githubusercontent.com/1278021/136975148-729588ff-808f-49b4-b575-31898b760bc1.png)

- Scroll down to the `Bounding Boxes` section, generate the `bboxes` and `Save`

    ![image](https://user-images.githubusercontent.com/1278021/136975424-89abcb3d-7f26-4d91-9b77-6d87fd934693.png)

## Publish the Layer into GeoNode

- Go back to the terminal, `activate` the `geonode` virtual environment and move to `/opt/geonode`

    ```shell
    workon geonode
    cd /opt/geonode
    ```

- Execute the `updatelayers` management command in order to publish the new layer from GeoServer

    ```shell
    ./manage_dev.sh updatelayers -h
    usage: manage.py updatelayers [-h] [-i] [--skip-unadvertised] [--skip-geonode-registered] [--remove-deleted] [-u USER] [-f FILTER] [-s STORE] [-w WORKSPACE] [-p PERMISSIONS] [--version] [-v {0,1,2,3}]
                                  [--settings SETTINGS] [--pythonpath PYTHONPATH] [--traceback] [--no-color] [--force-color]

    Update the GeoNode application with data from GeoServer

    optional arguments:
      -h, --help            show this help message and exit
      -i, --ignore-errors   Stop after any errors are encountered.
      --skip-unadvertised   Skip processing unadvertised layers from GeoSever.
      --skip-geonode-registered
                            Just processing GeoServer layers still not registered in GeoNode.
      --remove-deleted      Remove GeoNode layers that have been deleted from GeoSever.
      -u USER, --user USER  Name of the user account which should own the imported layers
      -f FILTER, --filter FILTER
                            Only update data the layers that match the given filter
      -s STORE, --store STORE
                            Only update data the layers for the given geoserver store name
      -w WORKSPACE, --workspace WORKSPACE
                            Only update data on specified workspace
      -p PERMISSIONS, --permissions PERMISSIONS
                            Permissions to apply to each layer
      --version             show program's version number and exit
      -v {0,1,2,3}, --verbosity {0,1,2,3}
                            Verbosity level; 0=minimal output, 1=normal output, 2=verbose output, 3=very verbose output
      --settings SETTINGS   The Python path to a settings module, e.g. "myproject.settings.main". If this isn't provided, the DJANGO_SETTINGS_MODULE environment variable will be used.
      --pythonpath PYTHONPATH
                            A directory to add to the Python path, e.g. "/home/djangoprojects/myproject".
      --traceback           Raise on CommandError exceptions
      --no-color            Don't colorize the command output.
      --force-color         Force colorization of the command output.
    ```

    ```shell
    ./manage_dev.sh updatelayers --skip-geonode-registered -u test_user1 -w geonode -f test_layer_3857
    ```

- Verify that the new layer has been created on GeoNode and it belongs to `test_user1`

    ![image](https://user-images.githubusercontent.com/1278021/136981502-26240587-ab50-40c7-98f1-8a433f9d81f2.png)

- Refresh the QGIS Desktop connection, load the new layer and try modifying the geometries

    ![image](https://user-images.githubusercontent.com/1278021/136982155-2843ceb2-3d69-4b2e-b2df-fb7fdf3b7e0a.png)

    ![image](https://user-images.githubusercontent.com/1278021/136982183-16c9de53-0b58-4133-a665-d7460ba7ce26.png)

- Verify on GeoNode that the geometries have been committed; **remember to clear the image browser cache in order to see the changes**

    ![image](https://user-images.githubusercontent.com/1278021/136982325-26660fe3-a2c3-4923-9976-332093efa7bb.png)


#### [Next Section: Optimizing, publishing and styling GeoTIFF data](OPTIMIZE_RASTER.md)
