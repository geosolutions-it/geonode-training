# Interacting With Other Users and Groups
The GeoNode platform allows you to communicate by message with other GeoNode users and groups of users.

You can also invite external users to join your GeoNode. In order to do that, click on the `Invite Users` link from the the `Profile` page (see `Updating the Profile` section) or from the `About` menu in the `navbar`.

You can invite your contacts typing their email addresses in the input field as shown in the picture below.

- Click on `Submit` to perform the action.
    
    ![image](https://user-images.githubusercontent.com/1278021/125321486-074e1500-e33d-11eb-88dc-79673284483a.png)

- In this specific case, you will get an error while trying to invte new users through this feature. This is because there's no valid `EMAIL SMTP` service configured on the Virtual Machine and therefore GeoNode does not know how to send e-mails. Notice also that you might end up getting an error whenever you specify an invalid e-mail address or domain.
    
    ![image](https://user-images.githubusercontent.com/1278021/125321558-1af97b80-e33d-11eb-900e-b5e966a2cab8.png)

- In order to move on with the exercises and examples, let's create a new user, called `test_user2` similarly to the `test_user1` we just created on the previous section:

    USERNAME: `test_user2`
    E-MAIL: `test_user2@test.geonode.org`
    PASSWORD: `test_user2`
    PASSWRD (Again): `test_user2`
 
    ![image](https://user-images.githubusercontent.com/1278021/125321581-2187f300-e33d-11eb-9b76-843a89eaf798.png)

## Viewing other Members Information
- Once created, explore the memebers registered to the system by clicking on the `People` link from the `About` menu
    
    ![image](https://user-images.githubusercontent.com/1278021/125321604-277dd400-e33d-11eb-92f3-e8ccb4d5d4c9.png)

- Login as `test_user2` and try to get the details of the member `test_user1`
    
    ![image](https://user-images.githubusercontent.com/1278021/125321628-2fd60f00-e33d-11eb-865e-a060f9fd630b.png)

- Now try to get the details of your own user, `test_user2`. Notice that if your user is not a `superuser` you won't be able to access private information about the other members. Nevertheless, be careful because GeoNode will use those information on the *contents metadata* whenever you publish something on the platform.
   
   ![image](https://user-images.githubusercontent.com/1278021/125321651-349ac300-e33d-11eb-8cef-0ba48cdab937.png)

- Now click on the `Groups` link from the `About` menu.
   
   ![image](https://user-images.githubusercontent.com/1278021/125321957-80e60300-e33d-11eb-890a-9616cf4c5c19.png)

- You will be able to access the list of all the available **Groups** currently present into the platform. Notice that by default every new member will be automatically registered as a member of the `Registered Members` group. By clicking on the group name, you will be able to access the group details page. A *Group* in GeoNode not only allows you to group together different members in order to easy the assignment of permissions to the resources (we will see this more in details in the next sections) and to message them (see below), but also to allow other users to recognize them as part of the same *organization* or *department*. In other words, this is a way to organize internally the members registered to the platform.
   
   ![image](https://user-images.githubusercontent.com/1278021/125321978-880d1100-e33d-11eb-8b29-9dba3dea978b.png)

- One interesting feature of the groups, also, is that you can easily access their `Activity` list, i.e. you can quickly recognize all the contents provided by the members of the group. Accessing the group `Activity` allows you to easily recognize the contents published by the group members. Of course you will be able to see only the public ones or the ones you have access to, as a single user or as a member of a group.
   
   ![image](https://user-images.githubusercontent.com/1278021/125321992-8cd1c500-e33d-11eb-811c-8a2a7c9f2bfa.png)

- Similarly it is possible to the whole `Activity` list. Again, you will be able to see only public resources or the ones you have access to.
   
   ![image](https://user-images.githubusercontent.com/1278021/125322033-98bd8700-e33d-11eb-8056-101283ef507c.png)

   ![image](https://user-images.githubusercontent.com/1278021/125322076-a5da7600-e33d-11eb-9dce-254f92b2d2c3.png)

- Similarly it is possible to your own `Activity` list. The `WMS GetCapabilities Document` link will allow you to generate an `XML` output **OGC Compliant** reporting only the layers belonging to the current user. In the next section we will go deeper into those concepts in order to better understand what *OGC Compliant* means and what a *Layer* is.
   
   ![image](https://user-images.githubusercontent.com/1278021/125322097-abd05700-e33d-11eb-8d05-66efabc6fa47.png)

   ![image](https://user-images.githubusercontent.com/1278021/125322110-affc7480-e33d-11eb-8856-84c6e819f174.png)

## Contatcing and Messaging other Members
- It is also possible to use an internal *messaging system* in order to quickly communicate with other registered members or groups. Click on the `Message User` link from the `Profile` page menu. As a `test_user2` try to send a message to `test_user1`
   
   ![image](https://user-images.githubusercontent.com/1278021/125321691-3f555800-e33d-11eb-8561-d2f3e0e1e56b.png)

- You `Inbox` folder contains the incoming messages.
   
   ![image](https://user-images.githubusercontent.com/1278021/125321720-467c6600-e33d-11eb-83ed-a62803c5c8b1.png)

- From the `Inbox` it is also possible to access all the messages available, including the ones sent by you.
   
   ![image](https://user-images.githubusercontent.com/1278021/125321751-4ed4a100-e33d-11eb-98ef-aca23ec28305.png)

- Now log out and log in again as `test_user1`
   
   ![image](https://user-images.githubusercontent.com/1278021/125321773-5300be80-e33d-11eb-929b-08673233c9c7.png)

- Notice the new messages incoming from your `Inbox`
   
   ![image](https://user-images.githubusercontent.com/1278021/125321788-572cdc00-e33d-11eb-88f3-00ac042decb9.png)

- Try also to *reply* to `test_user2` message
   
   ![image](https://user-images.githubusercontent.com/1278021/125321813-5dbb5380-e33d-11eb-99e9-f37a9f3419e3.png)

- There's also the possibiity to send a message to a whole *group*; in order to do that, start typying the group name instead the single user one
   
   ![image](https://user-images.githubusercontent.com/1278021/125321875-6d3a9c80-e33d-11eb-8139-b915c80ffad2.png)

- Notice how each member of the *group* will receive the message

   ![image](https://user-images.githubusercontent.com/1278021/125321921-762b6e00-e33d-11eb-963b-b3d3e8f0b7d2.png)

   ![image](https://user-images.githubusercontent.com/1278021/125321935-7af02200-e33d-11eb-9aa0-fa447b88d288.png)


#### [Next Section: Adding Data to GeoNode](ADDING_DATA_TO_GEONODE.md)
