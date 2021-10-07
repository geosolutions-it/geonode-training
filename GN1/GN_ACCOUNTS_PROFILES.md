# Accounts and User Profiles
In GeoNode many contents are public so unregistered users have read-only access to public datasets.

In order to create and edit contents on GeoNode, you need first to *sign in* and *log in* as a **Registered Member**.

GeoNode is primarily a *social platform*, thus a primary component of any GeoNode instance is the **user account**.

This section will guide you through account registration, updating your account information and preferences, connections with social networks and email addresses.

## Register to the platform as a new Member
To take full advantage of all the GeoNode features you need an user account.

Follow these step to create a new one:

 - From any page in the web interface, you will see a `Register` link. Click that link, and the register form will appear
     ![image](https://user-images.githubusercontent.com/1278021/125270714-3b5c1280-e30a-11eb-9ad6-cfa14a77af5d.png)

 - On the next page, fill out the form. Enter a user name and password in the fields. Also, enter an email address.
 
     ```django
     E-mail: test_user1@test.geonode.org
     Username: test_user1
     Password: test_user1
     Password (again): test_user1
     ```
   
     ![image](https://user-images.githubusercontent.com/1278021/125271344-ee2c7080-e30a-11eb-8a03-abac2bb148bd.png)
   
- You will be automatically logged in and redirected to the GeoNode home page. On a production system, with a correctly configured `EMAIL SMTP` service, an email will be also sent confirming that you have signed up. If no errors occur during the registration, you will be able to recognize the username on the top-right side of the page:

    ![image](https://user-images.githubusercontent.com/1278021/125273295-0f8e5c00-e30d-11eb-97bf-b0e88b100616.png)
 
- To logout click on the `Log out` link of the user menu.

    ![image](https://user-images.githubusercontent.com/1278021/125273494-44021800-e30d-11eb-95e8-3552578f6a4b.png)

- You have to confirm this action as described in the picture below.

     ![image](https://user-images.githubusercontent.com/1278021/125273589-5f6d2300-e30d-11eb-9865-a1133a677189.png)

## Updating the Profile
A regisetered account on GeoNode not only allows you to create and publish your own contents, but also to access private resources and collaborate with other members by editing the data and metadata of the existing ones.

Having a good metadata set associated to the datasets is something critical. Once the contents have been created into GeoNode, the platform will be able to expose them through a **Metadata Catalog** and allow external clients to search and explore them via different protocols.

GeoNode will use the information associated with your profile to enrich the contents metadata, thus allowing the clients to perform accurate queries against the catalog interface.

This is why it is a good practice to and **strongly recommended** to carefully edit your profile information as a first action.

### Editing the Profile information
 - Once logged in, click on the `username` from the top-rigt side of the navigation bar; a dropdown menu will list a set of useful menu links. Click on the `Profile` one
       ![image](https://user-images.githubusercontent.com/1278021/125289898-0a86d800-e320-11eb-8976-33aa172cdda7.png)

- Click on the `Edit Profile` link from the right-side menu; start entering some information as shown on the screenshot here below
     ![image](https://user-images.githubusercontent.com/1278021/125290551-b92b1880-e320-11eb-81a8-4788cd66a391.png)

- Once finished, click on `Update Profile` in order to persist the changes
     ![image](https://user-images.githubusercontent.com/1278021/125290731-ed063e00-e320-11eb-9775-54f21bca5e83.png)
       
Notice that your personal information will be updated accordingly and shown on the profile page.

- It is possible, eventually, to add also a personalized `Avatar` by clicking on the `Change Your Avatar` button from the `Edit Profile` page
     ![image](https://user-images.githubusercontent.com/1278021/125291821-1bd0e400-e322-11eb-8a1c-4f71a67cf1ca.png)

- Click on the `Choose file` button as shown below
     ![image](https://user-images.githubusercontent.com/1278021/125291928-37d48580-e322-11eb-839c-d0d88e21838a.png)

- Click on the button `Other Locations` and then `Computer`
     ![image](https://user-images.githubusercontent.com/1278021/125307877-bafcd800-e330-11eb-9f24-ddf35fafa526.png)

- Search for the `opt` folder and double click on it
     ![image](https://user-images.githubusercontent.com/1278021/125308072-e384d200-e330-11eb-8e47-c1034653330f.png)

- Keep navigating till `opt/data/sample_data/documents/images` and search for the file `flowers.jpg`; select the file and click on the `Open` button
     ![image](https://user-images.githubusercontent.com/1278021/125308459-352d5c80-e331-11eb-8f70-7c18fdb5a19c.png)

- Notice the file name appearing on the upload form; click on the `Upload New Image button`
     ![image](https://user-images.githubusercontent.com/1278021/125308651-5f7f1a00-e331-11eb-887f-18f0a55c95b8.png)

- Your current avatar will be updated accordingly; click then on the back button in order to move to the edit profile page
     ![image](https://user-images.githubusercontent.com/1278021/125308854-8d645e80-e331-11eb-8606-11bef4ba427a.png)

- The updated avatar will be displayed both on the edit profile page and, as a small icon, beside the user name
     ![image](https://user-images.githubusercontent.com/1278021/125308963-a5d47900-e331-11eb-9180-a7c12bc2191d.png)

#### [Next Section: Managing Favorite Shortcuts](USER_FAVORITES.md)
