# Introduction to Administering GeoNode (Basics)
- Sign in as `admin`

    ![image](https://user-images.githubusercontent.com/1278021/125767202-d47ae255-3f1e-4e7a-ae7b-2ff65f8c4469.png)

- Notice that as a `superuser` you will have more options available from the profile dropdown menu

    ![image](https://user-images.githubusercontent.com/1278021/125767479-6138efac-defe-4ccd-9001-8ba4a40bb3ad.png)

- Click on `Admin` in order to get redirected to the `Django Administration Dashboard`

    ![image](https://user-images.githubusercontent.com/1278021/125767714-138a24da-4aed-4ee7-936f-6cf11f3a95e9.png)


## Django Administration Dashboard
The dashboard is a special section accessible, at least on GeoNode, only to `superusers`.

Through the dashboard an `admin` can access special functions and can interact directly with the models stored on the DB.

This is particularly usefull when we need to recover from an unusual situation, for example a layer could not successfully finish the configuation or some error occurred in the meanwhile.

Notice that the dashboard is composed by several sections. In this introductionary module we will focus on some of them, the most common ones.

### Announcements
![image](https://user-images.githubusercontent.com/1278021/131642298-4c9959b2-cc4b-4f6b-8451-e2da1176295b.png)

This section allows an admin to create or change public announcements to the portal.

There are several types of announcements and those can be public, which means anyone will be able to see them, or `Members Only`, which means that only registered users will be able to see them.

**Important**: _Remember, when creating a new announcement, make sure the_ `Site Wide` _option is enabled_
      ![image](https://user-images.githubusercontent.com/1278021/131643156-15e23388-c3fb-4e2e-b05e-d3bfa0922af1.png)


### Base Administration
![image](https://user-images.githubusercontent.com/1278021/131643889-5991e033-125a-4bee-ba52-292205aaa25a.png)

This section allows an admin to customize some of the default Metadata contents of GeoNode, like:
 - Licenses
 - Topic Categories
 - Keywords
 - Metadata Regions

Not all the sections can be changed.

**Warning**: _Change those contents only if you know what you are doing!_

### People
![image](https://user-images.githubusercontent.com/1278021/131837708-c883e372-9ff8-4086-b9c6-483313dbfd03.png)

This section allows an admin to manage the system `users` (i.e. the registered members).

Through this panel it is possible to:

 - Create/modify/delete users
 - Diasable users or update/change their passwords
 - Promote a user to `superuser`

### Resources: Documents, Geoapps, Layers and Maps
![image](https://user-images.githubusercontent.com/1278021/131838173-cd425070-a5a1-480c-ad30-39446c44caf1.png)

This section allows an admin to manage the GeoNode `resources`.

Through this panel it is possible to:

 - Modify/delete resources
 - Publish/unpublish resources

### GeoNode Themes Library
![image](https://user-images.githubusercontent.com/1278021/131838461-984d9095-534f-41fa-896b-8045cb0cfa03.png)

This section allows an admin to manage the GeoNode `simple themes`.

For more informations please check the GeoNode documents section [Simple Theming](https://docs.geonode.org/en/3.x/admin/admin_panel/index.html#simple-theming).

#### [Next Section: Mastering GeoNode data publishing and management (GN2)](../GN2)
