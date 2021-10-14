# Charts and Widgets

- As `test_user1` create a new map and add the `ne_50m_admin_0_countries` layer

    ![image](https://user-images.githubusercontent.com/1278021/137338921-ebfb1bd5-b397-4066-9aa4-0f519dc451f5.png)

- Let's tweak a bit some settings as we learn from the previous section

    ![image](https://user-images.githubusercontent.com/1278021/137339225-bc85f514-0e64-447e-aa16-4d357b7fbc5d.png)

- Let's also change the style to the `CSS` one we created on the previous chapters

    ![image](https://user-images.githubusercontent.com/1278021/137339339-8ebb4d96-b1c5-424b-a2b7-234e84c2b6d1.png)

- Open the `Charts and Widget` window and select `Table`

    ![image](https://user-images.githubusercontent.com/1278021/137339547-4f053404-e804-4fec-bd0d-8a6a06aeb0a1.png)

- Select only the attrbutes `NAME` and `ISO3` and click on `Next` icon

    ![image](https://user-images.githubusercontent.com/1278021/137339711-973f9879-4ca3-4753-a33b-88c78c564fbc.png)

- Provide some `Title` and `Description` and `Save`

    ![image](https://user-images.githubusercontent.com/1278021/137339859-80a3355d-48b0-432a-b6ed-ea8edc7018a2.png)

- Resize and move the widget where you want on the map; also notice that its contents change accordingly to the geometries present on the viewport

    ![image](https://user-images.githubusercontent.com/1278021/137340019-a6757d92-c632-43b5-9246-82856d27f0ab.png)

- To change its contents and make them fixed, despite the zoom level, edit the widget again and uncheck the `Chain` icon

    ![image](https://user-images.githubusercontent.com/1278021/137340270-64e10d62-1a2a-4893-b187-f73d495d5b44.png)

- Let's add a `Chart` widget to the map; repeat the steps at point 1 but selecting `Chart`

    ![image](https://user-images.githubusercontent.com/1278021/137340457-877d664e-7f1f-4579-8ca0-b7c20ac36482.png)

- Select `ISO3` as `X Axis`, `POP_EST` as `Y Axis`, `MAX` as `Operation` and, optionally, change the color of the chart; notice the preview changing accordingly

    ![image](https://user-images.githubusercontent.com/1278021/137340874-fad8716d-7d23-4844-b483-f0af4751d3fb.png)

- Open the `Advanced Options`, change the `Type` to `LOG` and notice the scale and preview changing accordingly

    ![image](https://user-images.githubusercontent.com/1278021/137341426-2035b23f-b6b6-474b-8a94-5775c0967448.png)

- Try changing it to `CATEGORY` also

    ![image](https://user-images.githubusercontent.com/1278021/137341553-aebaae79-6251-4bf8-9b64-578c340cdaf3.png)

- Change it back to `LINEAR`, optionally add a `Prefix` and `Format`, check the preview and save

    ![image](https://user-images.githubusercontent.com/1278021/137341823-256b1a12-1422-442e-bcf5-75db93a91b3a.png)

- Bind the chart to the map and try zooming on some regions of `Africa`; notice the chart adapting accordingly

    ![image](https://user-images.githubusercontent.com/1278021/137342043-2d055699-2569-4380-a1c5-aeda7cd6e1b4.png)

- Try zooming around over some other continents and see how the chart changes

    ![image](https://user-images.githubusercontent.com/1278021/137342163-f9cc2678-7d0f-426a-8aa1-746cbe4cbd97.png)


#### [Next Section: Mastering GeoNode installation and configuration (GN3)](../../GN3)
