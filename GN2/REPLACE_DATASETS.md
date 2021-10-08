# Downloading, Replacing, Appending Data

## Downloading

If you have the permissions to do that, from the dataset detail page it will be possible to download it in several output format. The conversion will be performed automatically by GeoNode through the `OGC Protocols`.

- Move to the `Mainrd` layer

    `http://localhost/layers/geonode_data:geonode:Mainrd`
    
- Click on the `Download Layer` button; it will show up a small window with several options and choices

    ![image](https://user-images.githubusercontent.com/1278021/136572879-6dc826b5-5aba-4086-a97b-024d3dbebf7e.png)

- The first tab, `Images`, is not tipically much useful. It does not allow you to download any kind of _raw data_, but just some _portryals_ in several image format, which could be used eventually as thumbnails if needed. You can try out by clicking on some of the available image formats.

- The second tab, `Data`, allow you to download the whole dataset or a subset of it in some different download format.

### Downloading the whole dataset

From the `Data` section it is possible to download the _raw data_ into several `output formats`.

![image](https://user-images.githubusercontent.com/1278021/136574918-1114da80-4b0f-4b43-9354-50cbd44bd822.png)

 - The `Original Dataset` link, will allow you to retrieve the original files used to create the dataset the first time. This link might be unavailable in some cases:
     - The dataset has been created from the backend, i.e. it has not been uploaded from the `Upload Panel`
     - The original files are not available anymore on the file system
     - The administrator removed the link

 - Moreover the `Original Dataset` link differs from the others for two main things:
     1. It will always ignore any kind of filter appliead to the `Download Form` and it will provide the whole files.
     2. It may refer to some external repository in the case the administrator has changed it. In that case it will redirect the download of the files to some external storagre.

 - Other `download formats`; those ones are available accordingly to:
     1. The type of dataset; it is possible that some datasets could not be converted on some specific dataset (e.g. vectorial and rasters)
     2. Plugins availablity on `GeoServer`; the conversion in this case will be done by the backend on-the-fly. The available outputs, therefore, depend on their availability on the geospatial service.

### Obtaining a filtered subset

This option is valid only for `VECTORIAL` datasets.

- From the `Data` section it is possible to create and apply some filtering in order to get only a subset of the whole dataset.
- Click and enable the `Do you want to filter it?` button from the `Download` tab. It will take some time to parse the values from the backend.

    ![image](https://user-images.githubusercontent.com/1278021/136577215-072477e5-7e7b-4fdb-991e-9792a94e45bd.png)

- 

#### [Next Section: Editing Data](EDIT_DATA.md)
