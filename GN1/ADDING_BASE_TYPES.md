# Adding Base Types to GeoNode
This section explains how to add some of the base data types into GeoNode.
As an example we will learn how to upload a `ShapeFile` and `GeoTIFF` into GeoNode, as well as how to create maps and documents.

## Adding a Shapefile
 - Navigate to the GeoNode main page `http://localhost` and log in as `test_user1`; from the `navbar` click the `Upload Layer` link from the `Data` dropdown menu
     ![image](https://user-images.githubusercontent.com/1278021/125445688-c9d94fd3-885d-43a1-bdb9-123b046c8ac3.png)

 - In the next page, select and click the `Choose Files` button
     ![image](https://user-images.githubusercontent.com/1278021/125445932-4a3b90b5-4290-4b6f-a095-05dbea5bcf55.png)

 - From the `file browser window` navigate to:
    * `/opt/data/sample_data/pretty_maps/data/boulder`
    * By pressing `RIGHT-SHIFT` plus `LEFT-CLICK` select all the `Mainrd.*` files
    * Click on the `Open` button
    
      ![image](https://user-images.githubusercontent.com/1278021/125446381-998a074c-46a8-4cb6-9cf1-171576173038.png)

 - If everything went well, you should be able to see the files listed into the `Upload Layer` page; click on the `Upload files` button
     ![image](https://user-images.githubusercontent.com/1278021/125447121-e0ba80d7-3cc6-4f47-b238-46fb85bf4034.png)

 - The upload will start and you will be able to see a progress bar on the top of the page; please be patient, if you have a slow machine it will require some time to finish
     ![image](https://user-images.githubusercontent.com/1278021/125447343-cb0a152a-2353-47fd-bb84-c750e7973dc8.png)

 - If no errors occur, the progress reaches the `100%` and the name of the new `Layer` becomes clickable
     ![image](https://user-images.githubusercontent.com/1278021/125447446-0507fdbc-b72e-4026-9277-f1f3e00629a2.png)

 - Click on the `Layer` name in order to be redirected to the `Details Page`. This page is composed by different sections. We will inspect them in details in the next chapters.
     ![image](https://user-images.githubusercontent.com/1278021/125447600-60ec1eb7-9d8a-4b54-b276-2669e9d7a79c.png)

 - Click on the `Layers` link of the `Data` dropdown menu in order to go back to the list of all the available layer.
   The newly uploaded layer will be listed and a small summary card will report the most important information about that:
   
     * A thumnnail depicts a quick preview of the layer
     * A small section reports the clickable name of the layer along with an abstract, which by default is empty
     * There's the name of the owner/creator of the content as well as the publication date
     * Some social shortcuts allow us to understand how many times the layer has been viewed or downloaded
     * Finally a shortcut link would allow us to create an empty map by using this layer as a overlay
     
     ![image](https://user-images.githubusercontent.com/1278021/125448713-ced1589b-5cd3-4b8e-942a-c5507e24f9ae.png)

## Adding a GeoTiff
 - Repeat the first steps we have doen for the `Shapefile` upload, select and click the `Choose Files` button
     ![image](https://user-images.githubusercontent.com/1278021/125445932-4a3b90b5-4290-4b6f-a095-05dbea5bcf55.png)

 - From the `file browser window` navigate to:
    * `/opt/data/sample_data/user_data/aerial`
    * Click on the `search` button
 
    ![image](https://user-images.githubusercontent.com/1278021/125457589-204a9730-a8bd-48f3-a28b-bd2d353eae52.png)

 - In the `search textbox` insert the name `13tde815295_200803_0x6000m_cl.tif`
    * By pressing `RIGHT-SHIFT` plus `LEFT-CLICK` select all the `13tde815295_200803_0x6000m_cl.tif` file
    * Click on the `Open` button
 
     ![image](https://user-images.githubusercontent.com/1278021/125457743-a13b4a5e-6c73-45ab-a39a-901b999c1156.png)

- Upload the file and wait for the progress bar to finish
    ![image](https://user-images.githubusercontent.com/1278021/125457990-a2bf13a1-c79b-45b1-8b2e-fe19165499fb.png)

- Move to the layers list and select the newly created layer
    ![image](https://user-images.githubusercontent.com/1278021/125458058-272488cb-505b-478e-a167-3bbe532052f7.png)

- Make sure you can navigate the map preview from the layer details page
    ![image](https://user-images.githubusercontent.com/1278021/125458137-c7ede57e-af4c-4ae4-a3eb-f3437976d0d3.png)

#### [Next Section: Styling with GeoNode](STYLING_BASE.md)
