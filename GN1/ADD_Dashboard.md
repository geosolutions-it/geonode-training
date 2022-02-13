# Adding our first Dashboard
A `Dashboard` is a space where a user can add many miniature items, such as charts, maps, tables, texts and counters, and can create connections between them in order to provide an overview for a better visualization of the data, interact spatially and analytically with the data by creating connections between maps and charts and/or perform analysis for better decisions.

## Add world dataset and create a simple map
Like in previous upload layers steps, follow the path to `/opt/data/sample_data/pretty_maps/data` and upload the shapefile with name `ne_50m_admin_0_countries`.

Once upload finishes, layer detail page will look like this:

![image](https://user-images.githubusercontent.com/1199224/152556782-c6dbcc16-105d-4f25-bc43-6de4dd4b200c.png)


## Create a Dashboard
From the menu `Apps` follow the link `Explore Apps`

![image](https://user-images.githubusercontent.com/1199224/152551552-29a8a36d-d3be-42a5-b577-0866d7c64649.png)

Once on the Apps list page click the button `Create New Apps` on top right corner and select `Dashboard` from the drop-down menu.

![image](https://user-images.githubusercontent.com/1199224/152552213-e094f06b-ad93-4d0d-8f2b-d1b058b4015e.png)

You will be presented an empty dashboard page ready to be populated.

Click on the Add icon ![image](https://user-images.githubusercontent.com/1199224/152554109-d03a670b-61cf-47ee-80ec-5049c511738b.png)
to add new widgets.

![image](https://user-images.githubusercontent.com/1199224/152554228-1efcb687-f132-496c-a5ec-e08ed999ad1b.png)

The `Widget` page will open on the left showing a list of items that can be created and added to the dashboard.

![image](https://user-images.githubusercontent.com/1199224/152554342-fe3af6e5-dc74-4d4f-a4c8-38a7d760391d.png)

## Add map widget
Let's add our first widget to our dashboard, the `Map` widget type

Click the `Map` widget button and you'll be redirected to a list of GeoNode existing maps. Choose our previously created `World Population` map and click the right arrow ![image](https://user-images.githubusercontent.com/1199224/152818038-fda37976-f141-4eea-b2aa-78e8b3635c49.png) to add this to our dahsboard.

Once added, you can configure some map options, such as info tool disable/enable ![image](https://user-images.githubusercontent.com/1199224/152818723-7267e11c-8c12-4a5b-b10b-bcbc25672a94.png)
 or add other layers to the existing map using the plus ![image](https://user-images.githubusercontent.com/1199224/152818610-49f58b99-4b88-4a95-90e0-38f94f774d65.png) button.

For now let's just proceed without changing the map options by clicking again the right arrow button ![image](https://user-images.githubusercontent.com/1199224/152818038-fda37976-f141-4eea-b2aa-78e8b3635c49.png)

On the last step for creating the `Map` dashboard widget you are asked to provide a `Title` and a `Description` for you widget. Add there some relevant information as below:

![image](https://user-images.githubusercontent.com/1199224/152819409-485fb432-8cad-4175-845d-0635685d6f89.png)
 
Finally save it using the floppy disk button ![image](https://user-images.githubusercontent.com/1199224/152820031-32d85f4f-82f8-4d99-ab03-be239c19b430.png)

Now we can see our `Map`widget on our dashboard. Resize the widget so that it can cover the most of the area of your dashboard. Just leave some space at the botton and to the right of the dashboard canvas area as shown below:

![image](https://user-images.githubusercontent.com/1199224/152820284-9a16e1d9-e4ba-4f1c-b4a8-8248698fb351.png)

## Add table widget
At the botton of our dashboard canvas we will include another type of widget, the `Table` type widget.

Click again on the add widget button ![image](https://user-images.githubusercontent.com/1199224/152554109-d03a670b-61cf-47ee-80ec-5049c511738b.png) to add one more widget to our dashboard.

From the widget list, select the `Table` widget type.

Once selected your will be redirected to GeoNode list of available layers. If you did not renamed previously our world population layer, choose the layer `ne_50m_admin_0_countries0`layer from the list.

![image](https://user-images.githubusercontent.com/1199224/152821472-b24f3db7-4f66-4276-8016-c3cf76a7e38e.png)

Once selected, click againg the right arrow button ![image](https://user-images.githubusercontent.com/1199224/152818038-fda37976-f141-4eea-b2aa-78e8b3635c49.png) to proceed.

On the next step you will be asked which fields you want to show on the dashboard table widget. In this case we will only choose fields, `NAME`, `POP_EST` and `ISO_A2`  

![image](https://user-images.githubusercontent.com/1199224/152822350-01e3328b-1f00-4c24-86b1-3ac68e929af7.png)

Before proceeding, please also enable the `connect widget` button ![image](https://user-images.githubusercontent.com/1199224/152822609-586b23a4-1d25-4c0b-9918-7436057967a9.png) by clicking on it.

Once again in the last step we'll need to provide a title and description for our widget.

![image](https://user-images.githubusercontent.com/1199224/152822906-a79fc304-3c9a-4eb7-ad06-0f739820c7da.png)

At the end, click again the floppy disk button ![image](https://user-images.githubusercontent.com/1199224/152820031-32d85f4f-82f8-4d99-ab03-be239c19b430.png) to save and add the widget to our dashboard.

Place it at the bottom of our dashboard and change the widht of it to cover all the bottom area.

![image](https://user-images.githubusercontent.com/1199224/152833312-0f192785-cb00-4d6c-b7b3-2c21fa77a6ee.png)

## Add legend widget
Next we will add a legend widget to our dashboard in order to make the map color classes more readable.

Click on the plus button ![image](https://user-images.githubusercontent.com/1199224/152554109-d03a670b-61cf-47ee-80ec-5049c511738b.png) to add a new widget and from the list choose `Legend` widget type.

You should see the legend for our world population map layer (`ne_50m_admin_0_countries0`) on the preview of the legend widget area.

![image](https://user-images.githubusercontent.com/1199224/152834073-1b79dead-e420-488d-83fd-4c8a02d57790.png)

Let's move on by clicking the right arrow button ![image](https://user-images.githubusercontent.com/1199224/152818038-fda37976-f141-4eea-b2aa-78e8b3635c49.png).

Again, the final step is to provide a Title and a Description to our widget.

![image](https://user-images.githubusercontent.com/1199224/152834485-d8ea4939-833a-47d1-bb4f-10cb2e08ca7b.png)

Once done, save it and add the widget to the map by clicking the floopy disk button ![image](https://user-images.githubusercontent.com/1199224/152820031-32d85f4f-82f8-4d99-ab03-be239c19b430.png).

Move the widget and place it near the empty area to the right of our existing map. Adjust widgets sizes if needed to have a dashboard configuration similiar to the next picture.

![image](https://user-images.githubusercontent.com/1199224/152835075-0499af83-2754-4a9c-b52d-e45becaa1108.png)

## Add counter widget
As a final trial to our dashboar we will include a `Counter`widget type in order to provide the total number of countries in the world.

From the add more widget button ![image](https://user-images.githubusercontent.com/1199224/152554109-d03a670b-61cf-47ee-80ec-5049c511738b.png) choose the widget type `Counter` from the list.

Again, it will show a list of available layers to choose from. Choose again the `ne_50m_admin_0_countries0` layer.

![image](https://user-images.githubusercontent.com/1199224/152835550-05250d31-a53e-4d6f-adc2-5d9cd187c2cb.png)

Let's move on by clicking the right arrow button ![image](https://user-images.githubusercontent.com/1199224/152818038-fda37976-f141-4eea-b2aa-78e8b3635c49.png).
Now we need to configure our widget and from the configuration page we can see:
* `Use`- The property/column field to use for our calculation
* `Operation`- The available field level calculation available (`COUNT`, `SUM`, `AVG`, etc...)

Let's use the field `fid` and the operation `count` for this exercise.

![image](https://user-images.githubusercontent.com/1199224/152836303-9f41da44-8f85-495f-b767-accdf90d63e9.png)

You should see already in the preview the total number of countries in the layer (`242`)

Click the right arrow button ![image](https://user-images.githubusercontent.com/1199224/152818038-fda37976-f141-4eea-b2aa-78e8b3635c49.png). and again add a title and description to our configured widget.

![image](https://user-images.githubusercontent.com/1199224/152836969-a302239e-8481-4a05-8928-6e08c1377cae.png)


Final step is to save the widget and place the widget on our dashboard using the floppy disk button ![image](https://user-images.githubusercontent.com/1199224/152820031-32d85f4f-82f8-4d99-ab03-be239c19b430.png). Feel free to place it somewhere as you will need to rearrange other existing widgets to allow our new `Counter` widget to be clearly visible.
![image](https://user-images.githubusercontent.com/1199224/152837179-65f4ae59-6f7f-458e-b155-0b81f5176395.png)


**Congratulations**!! You have built your first simple dashboard with GeoNode!
