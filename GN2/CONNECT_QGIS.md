# Accessing Data from External Clients (QGIS)

## Install QGIS Desktop

- Open the `Ubuntu Software` app

    ![image](https://user-images.githubusercontent.com/1278021/136914738-bcf7ebc2-6420-47c2-b876-f982945f0f56.png)

- Click `CTRL + F` and search for `qgis`; click on the `QGIS Desktop` icon

    ![image](https://user-images.githubusercontent.com/1278021/136914846-95358fc2-aa9b-4a4d-92f2-ab2e2548e484.png)

- Click on `INSTALL` and wait for the process to finish

    ![image](https://user-images.githubusercontent.com/1278021/136915019-bb082b2e-4372-4d4d-b8cd-614b32e0ac50.png)

- Once the app has been installed, open it by clicking on the icon

    ![image](https://user-images.githubusercontent.com/1278021/136915075-6496ba07-3af7-45b1-9cdc-2d93530a0920.png)

## Connect through BASIC Auth
This is the easiest way to connect the client to GeoNode:

    - Pros: very easy to configure
    - Cons: it uses always a fixed user and you need to change it anytime if you want to switch it

- Let's add a `VECTORIAL` layer accessible to `test_user1` to the client; click on `Layer > Add Layer > Add WFS Layer...`

    ![image](https://user-images.githubusercontent.com/1278021/136917776-624696aa-c345-43d8-a117-f9e9bcc49b67.png)

- Create a `New Connection`

    ![image](https://user-images.githubusercontent.com/1278021/136917846-a27e5109-eede-44ef-b695-7e0e8f990e56.png)

- Provide a name, e.g. `GeoNode WFS` and the following URL:

    - `http://localhost/gs/ows`
    
    ![image](https://user-images.githubusercontent.com/1278021/136918114-c9bd2c31-75d0-46cd-abae-5f6f48098fb8.png)

    _IMPORTANT: It is mandatory to pass through the GeoNode proxy_ `/gs/` _instead of hitting the GeoServer endpoint directly_

- If the client asks for a `NEW master password` you can just provide anyone, e.g. `geonode`

    ![image](https://user-images.githubusercontent.com/1278021/136918608-ce4f5b10-433a-43e4-8b2f-68b3f005865d.png)

- Switch to `Basic` authentication, provide the `test_user1` credentials and click on `Convert to configuration`

    ![image](https://user-images.githubusercontent.com/1278021/136922761-ff3890b1-7781-4ce9-986f-8cfe11c553bf.png)

- Make sure the converted configuration is selected and click on `Detect` in order to verify that it works; click on `OK` when finished

    ![image](https://user-images.githubusercontent.com/1278021/136918990-c2d355c6-94e1-4a32-b126-986b40549327.png)

## Connect through OAuth2
This is the easiest way to connect the client to GeoNode:

    - Pros: difficult to configure
    - Cons: it redirects to GeoNode to authenticate, so you can use any login provided by GeoNode

- We need to prepare GeoNode first; as an `admin` go to the `Admin Dashboard` and look for `Django OAuth Toolkit > Applications`

    ![image](https://user-images.githubusercontent.com/1278021/136919833-371ebe26-03fc-4f96-bd8a-3cb304493361.png)

- Edit the `GeoServer` one

    ![image](https://user-images.githubusercontent.com/1278021/136919931-53a0ee34-4687-4281-a422-c4cfe2c7cccf.png)

- Add the following URL to the `Redirect URIs` section and take note of the `Client ID` and `Client Secret` keys:

    * **Copy the Client ID / Client Secret**

    ![image](https://user-images.githubusercontent.com/1278021/136920356-99b64556-6bf7-440b-99dd-0c1e9f3d4014.png)

    * **Add Redirect URIs and Save**: `http://127.0.0.1:7070/qgis-client`

    ![image](https://user-images.githubusercontent.com/1278021/136920406-53eb70a3-d2ae-4ed9-b108-20be60c29223.png)


- Let's add a `VECTORIAL` layer accessible to `test_user1` to the client; click on `Layer > Add Layer > Add WFS Layer...`

    ![image](https://user-images.githubusercontent.com/1278021/136917776-624696aa-c345-43d8-a117-f9e9bcc49b67.png)

- Add a new `OAuth2 Authentication` config and fill the fields as follows:

    * **Name**: _Provide any name you want e.g._ `GeoNode OAuth2`
    * **Grant Flow**: `Authorization Code`
    * **Request URL**: `http://localhost/o/authorize/`   (**the** `/` **at the end is IMPORTANT!**)
    * **Token URL**: `http://localhost/o/token/`   (**the** `/` **at the end is IMPORTANT!**)
    * **Refresh token URL**: `http://localhost/o/token/`   (**the** `/` **at the end is IMPORTANT!**)
    * **Client ID / Client Secret**: _The ones above_
    * **Scope**: `openid write`
    * **Token session**: `True`
    * **Access method**: `Header`
    * **Token header**: _empty_  (**it is important you leave this param empty*)*
    * **Save**
    
    ![image](https://user-images.githubusercontent.com/1278021/136948695-5310e7fb-9bac-46c7-a32d-2843be89041c.png)

- Make sure the new configuration is selected and click on `Detect` in order to verify that it works; click on `OK` when finished

    ![image](https://user-images.githubusercontent.com/1278021/136923742-a0fa4b95-bc66-4f2c-97af-f1bd6efc53d5.png)
    
- The client will open automatically a browser session, if you are not logged in, sign in with `test_user1`

    ![image](https://user-images.githubusercontent.com/1278021/136923067-147aa88a-96d5-4a46-ba7f-053162efc283.png)

- The window below means that the authetnication process was successfull; you can safely close it and go back to the client

    ![image](https://user-images.githubusercontent.com/1278021/136923358-3b54e963-49b2-4e78-9bcd-2fb4512a7c9d.png)

## Attach Layer to the Project

- Once the connection has been configured and saved, whatever it is, go back to the `WFS` panel, select the connection you just created and click on `Connect`

    ![image](https://user-images.githubusercontent.com/1278021/136935422-d297ae37-b25b-4401-a796-bb1ba1bb29f5.png)

- If everything goes weel, you should be able to see the server offering; it will list all the layers the user has access to

    ![image](https://user-images.githubusercontent.com/1278021/136935553-b5db6117-bcc5-4d14-b48e-33bc6f87118f.png)

- Select the `Test Layer` and click on `Add`

    ![image](https://user-images.githubusercontent.com/1278021/136935660-1c67970b-ddcb-40c4-a10c-593cf098b98b.png)

- QGIS will create a new project with the layer already loaded and centered to the map

    ![image](https://user-images.githubusercontent.com/1278021/136950197-9430ba5e-008a-410d-abf3-6f797c08d892.png)

## Editing Contents: Values

- Enable `Editing Mode` on QGIS and click on the `Info` button

    ![image](https://user-images.githubusercontent.com/1278021/136950557-79ebfe19-42e7-4bc2-a570-e3284d3bce33.png)
    
- Click over the geometry to edit and, on the right panel, expand and click on the link `Edit feature form`

    ![image](https://user-images.githubusercontent.com/1278021/136950920-6a177c0e-58fa-4229-8852-0e7b20c95e56.png)

- That will show a small form with the values, change few ofthem and click on `OK` button

    ![image](https://user-images.githubusercontent.com/1278021/136951126-0761cac6-7ba5-4517-bcbf-5b7c2ad976f9.png)

- A small `floppy disk` button will pop near the editing one meaning that there are some pending changes to be committed to the server; click on it in order to persist the changes

    ![image](https://user-images.githubusercontent.com/1278021/136951347-2b405b41-6f09-49cf-9e2e-3d56b3b6585a.png)

- At a successfull commit, the `floppy disk` button will be disabled again

## Editing Contents: Geometries

- Enable `Editing Mode` on QGIS and click on the `Info` button

    ![image](https://user-images.githubusercontent.com/1278021/136950557-79ebfe19-42e7-4bc2-a570-e3284d3bce33.png)

- Click on the `Vertex Tool` and enable it; from now on by moving over a geometry you will be able to modify its vertices

    ![image](https://user-images.githubusercontent.com/1278021/136952225-3827b983-f192-4285-a80a-facc85af2d33.png)

- Once happy with the changes, save them like we have done previously on the values

    ![image](https://user-images.githubusercontent.com/1278021/136952375-5fe8775a-f7da-4455-b2f8-6dfe7ba2bb57.png)

- With this specific layer most probably you will get an error on the bounding box extension; this is caused by the native projection of the layer and the QGIS not being able to correctly manage the `dateline`

    ![image](https://user-images.githubusercontent.com/1278021/136952674-ef76f8f3-1b0b-4a9d-a8fc-00832599ea11.png)

- It is still possible to edit the layer from GeoNode directly, however in order to fix this issue easily, we will convert the layer into a `Mercator Projected` one.

We will pass through the database in order to perform such operation. In the next section we will see how to re-project and store and a DB table a layer and then push it back to GeoNode.
    
#### [Next Section: Advanced layers publishing and management](ADV_LAYERS_PUB.md)
