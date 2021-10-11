# Creating and Editing Data

## Creare an Empty Layer

- Click on `Data > Create Layer`

    ![image](https://user-images.githubusercontent.com/1278021/136794231-d271f26f-7b67-4c83-b935-a09152245ee6.png)

- Fill the form with some values; first of all insert a `name` and `title` and select `Polygon` as `geometry type`
- Add three `attributes` and click the `Create` button

    - `label`: `String`
    - `value`: `Float`
    - `date`: `Date`

    ![image](https://user-images.githubusercontent.com/1278021/136794405-44b4dcb2-2331-4841-ae95-d3294dcd6b84.png)

- GeoNode will create the new empty layer with the selected `geometry type` and `attributes` and will redirect you directly to the detail page

    ![image](https://user-images.githubusercontent.com/1278021/136794712-2e65aded-a568-467c-a32d-c7e6c75ea1ee.png)

## Edit the Layer Data

- Click on `Editing Tools > Layer > Edit Data`

    ![image](https://user-images.githubusercontent.com/1278021/136794790-97b73583-5827-42af-821c-f324b3c54756.png)

- Go to `Boulder` area by using the `geocoding` widget

    ![image](https://user-images.githubusercontent.com/1278021/136794887-36e49940-1f6c-4d2c-905a-c839164d27c1.png)

- Click on the `pencyl` in order to enter the `edit mode`

    ![image](https://user-images.githubusercontent.com/1278021/136794969-d6e32411-1bc9-42fd-98e5-d0e46bd46eac.png)

- Click on the `add new geometry` button

    ![image](https://user-images.githubusercontent.com/1278021/136795049-c93854f8-7dcb-4958-98da-7a6febdb999e.png)

- Click on the `Pencyl` again

    ![image](https://user-images.githubusercontent.com/1278021/136795170-a7f51c1c-c5c9-44bd-a6c5-585e9d34416c.png)

- Draw a small `polygon` over the `Boulder Airport` and click on the `floppy disk` button

    ![image](https://user-images.githubusercontent.com/1278021/136795277-1d3ec176-9e5c-4a81-b48c-3b0d6f674fc0.png)

- Double-click on the `attributes table` columns in order to enable the `edit mode` on the values

    ![image](https://user-images.githubusercontent.com/1278021/136795409-290392be-eea8-4638-b26f-18798ebdc1e2.png)

- Insert some values as shown below; **the date-time attributes must be provided in ISO format**

    ![image](https://user-images.githubusercontent.com/1278021/136795526-946c9611-3975-4835-bab3-e448d0f0f32b.png)

- Save and repeat the operation as many times you want

    ![image](https://user-images.githubusercontent.com/1278021/136795612-8ee461b9-2a87-4ef4-83ab-c77679573799.png)

- Once finished, go back to the layer detail page; you will notice that GeoNode will show some features but the bounding box is still the whole world; **remember to clean the image cache of the browser in order to show the new features**

    ![image](https://user-images.githubusercontent.com/1278021/136795795-65687425-f26d-43f3-9844-2e3c9c26df66.png)

## Update the Layer Bounding Box

- Logout and login again as an `admin`; from the context menù, click on `GeoServer`

    ![image](https://user-images.githubusercontent.com/1278021/136795982-869fc5c7-25e8-486e-8065-b352d42aeeee.png)

- If you are still logged in as `test_user1` on GeoServer, follow the steps below

    ![image](https://user-images.githubusercontent.com/1278021/136796113-323240a7-e8f0-42a2-b584-74d60e30a2cb.png)

    ![image](https://user-images.githubusercontent.com/1278021/136796156-ca743cd8-57a7-4287-afc9-62a5646e8ae9.png)
    
    ![image](https://user-images.githubusercontent.com/1278021/136796188-0264d2b3-1412-4767-819d-8a88fd24658c.png)

    ![image](https://user-images.githubusercontent.com/1278021/136796221-0e629dca-5eb7-449d-b8de-d4b8ab6b1ba1.png)

- Go to the GeoServer `Data > Layers` section

    ![image](https://user-images.githubusercontent.com/1278021/136796292-4d19bb41-b33b-4179-bee0-1b8161124e3e.png)

- Select and click on the `Test Layer` you just created

    ![image](https://user-images.githubusercontent.com/1278021/136796367-c7851e93-fdb3-4ac7-83b7-dd8a051ddd7a.png)

- Scroll down to the `Bounding Boxes` section, update them as shown in the figure, and `Save`

    ![image](https://user-images.githubusercontent.com/1278021/136796467-957ec721-2266-4477-9372-b792a92f41eb.png)

- Go back to GeoNode; from the layer details page click on the `Refresh Attributes and Statistics` button

    ![image](https://user-images.githubusercontent.com/1278021/136796618-2f4698d1-a40a-4abe-b14f-c840a72535b2.png)

- The page will be refreshed and now the layer will be zoomed on the right location

    ![image](https://user-images.githubusercontent.com/1278021/136796725-ce1539f0-582f-420d-9d8e-474ea1096165.png)

## Delete Data

- Enter the `Edit Data` mode again

    ![image](https://user-images.githubusercontent.com/1278021/136796934-488d978b-7491-4317-b30d-e772dc31b08f.png)

- Select (click over) a polygon and click on the `Trash Bin` icon

    ![image](https://user-images.githubusercontent.com/1278021/136797009-be26b736-0bb7-46a2-acc3-da26185249f4.png)

- Confirm the deletion and verify the polygon disappear from the map

    ![image](https://user-images.githubusercontent.com/1278021/136797071-76fc36c5-9a79-444c-9c99-7a1163aba0e9.png)

- Go back to the layer details page, **refresh the browser image cache**, and verify the feature is not present anymore

    ![image](https://user-images.githubusercontent.com/1278021/136797204-2ea70636-de21-4c0b-b41f-9bae77513b1f.png)

## Refresh the Thumbnail

- Last thing to do is to refresh the layer thumbnail

    ![image](https://user-images.githubusercontent.com/1278021/136797284-00eed17a-27ee-4587-8fbf-a782b04afae1.png)

    ![image](https://user-images.githubusercontent.com/1278021/136797334-026198a6-e0d2-49c3-af85-5c4de19fc4c8.png)    

#### [Next Section: Downloading, Replacing, Appending Data](REPLACE_DATASETS.md)
