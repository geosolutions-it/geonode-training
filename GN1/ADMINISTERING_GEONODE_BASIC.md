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

#### [Next Section: Mastering GeoNode data publishing and management (GN2)](../GN2)
