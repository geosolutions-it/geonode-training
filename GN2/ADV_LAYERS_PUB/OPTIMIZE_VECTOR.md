# Optimizing, publishing and styling Vector data

## Styling with CSS
The CSS extension module allows to build map styles using a compact, expressive styling language already well known to most web developers: `Cascading Style Sheets`.

The extension is available on GeoServer only. The standard CSS language has been extended to allow for map filtering and managing all the details of a map production. In this section we'll experience creating a few simple styles with the CSS language.

## Styling vector data

### Creating line styles

- Go to the `mainrd` layer details page and click on `Editing Tools > Style > Edit`

    ![image](https://user-images.githubusercontent.com/1278021/137169042-03336eb9-efb9-4e17-9f52-1397d02105c8.png)

- Add a new `Style`; select `CSS` and click on the `+` button

    ![image](https://user-images.githubusercontent.com/1278021/137169285-5f30dd0a-63c8-4104-9469-a3df253652af.png)

- Name it `CSS Roads` and `Save`

    ![image](https://user-images.githubusercontent.com/1278021/137169534-eb271e8b-6e14-41bd-add9-3c34e43679a9.png)

- Edit the new style

    ![image](https://user-images.githubusercontent.com/1278021/137169696-0508d696-63e3-4dbf-946c-641b0159dac1.png)

- Switch to `text mode` and insert the following `CSS`

    ```css
    * {
      stroke: orange;
      stroke-width: 6;
      stroke-linecap: round;
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137170156-cfb0e829-f91d-47b3-968b-087dfb1af929.png)

- Now let’s create a cased line effect by adding a second set of colours and widths, and forcing two different `z indexes` for them. Rewrite the `CSS` as follows

    ```css
    * {
      stroke: orange, yellow;
      stroke-width: 6, 2;
      stroke-linecap: round;
      z-index: 1, 2;
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137170536-80bbba1d-bc52-4b03-8464-b84b5f313a91.png)

    _Notice the two strokes of different size look like the border of a highway._
    
- Let’s add a label that follows the road. Rewrite the `CSS` as follows

    ```css
    * {
      stroke: orange, yellow;
      stroke-width: 6, 2;
      stroke-linecap: round;
      z-index: 1, 2;
      label: [LABEL_NAME];
      font-fill: black;
      font-family: Arial;
      font-size: 12;
      font-weight: bold;
      halo-color: white;
      halo-radius: 2;
      -gt-label-follow-line: true;
      -gt-label-group: true;
      -gt-label-repeat: 400;
      -gt-label-max-displacement: 50;
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137170958-ec2fe6ed-05d6-47e2-8ba4-5ae5e77b097a.png)

    _Notice how compact is the CSS syntax versus the SLD one. It is also much more intuitive. While writing the CSS the text editor is also able to hilight typeahead hints._

### Creating point styles

- Go to the `pointlm` layer details page and click on `Editing Tools > Style > Edit`

    ![image](https://user-images.githubusercontent.com/1278021/137171852-ecd4ea8c-d124-4446-966b-b20192f331f6.png)

- Add a new `Style`; select `CSS` and click on the `+` button
- Name it `CSS Landmarks` and `Save`

    ![image](https://user-images.githubusercontent.com/1278021/137172026-293df006-12c8-4689-90d0-9fbe4e4c5cb1.png)

- Edit the new style
- Switch to `text mode` and insert the following `CSS`

    ```css
    * {
      mark: symbol('circle');
      mark-size: 5;
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137172171-9205d228-d60f-409e-b1ff-3ae8947163e2.png)

- Let’s change the color of the points by specifying a `fill`. If we specified a `fill` in the top level rule it would be interpreted as a polygonal fill, to express that we want to _fill inside the marks_ we have to create a **new rule** with the `:mark` pseudo-selector

    ```css
    * {
      mark: symbol('circle');
      mark-size: 5;
    }

    :mark {
      fill: cyan;
      stroke: darkblue;
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137172493-409482e9-d6f3-4918-8a62-9e177b5002c5.png)

- Finally, let’s override the default styling for all _shopping centers_. Shopping centers are not easy to find, they have a `MTFCC` category value equal to `C3081` and contain `Shopping` in the name.

    ```css
    * {
      mark: symbol('circle');
      mark-size: 5;
    }

    :mark {
      fill: cyan;
      stroke: darkblue;
    }

    [MTFCC = 'C3081' AND FULLNAME LIKE '%Shopping%'] {
      mark: url("./img/landmarks/shop_supermarket.p.16.png");
      mark-size: 16;
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137173141-f061ff81-958f-4fe3-9df2-e050d616d8c3.png)

### Creating polygon styles

- As `test_user1`, upload a the `ESRI Shapefile` named `ne_50m_admin_0_countries` from the folder `/opt/data/sample_data/user_data`

    ![image](https://user-images.githubusercontent.com/1278021/137174616-be359469-fcec-4fa8-b0ca-eb559b9ca774.png)

- Add a new `CSS` style to the new layer names `CSS Population`

    ![image](https://user-images.githubusercontent.com/1278021/137174881-2e409290-84d4-4f49-a31f-92c972522ace.png)

- We want to create a simple _3 class thematic_ map based on the country population, stored in the `POP_EST` attribute

    ```css
    [POP_EST < 10000000] {
      fill: lightgrey;
    }

    [POP_EST >= 10000000 AND POP_EST < 50000000] {
      fill: olive;
    }

    [POP_EST > 50000000] {
      fill: salmon
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137175256-3c90e87b-e669-4f70-ac6a-b948af9a727e.png)

- Let’s also add a very thin black border around all polygons, regardless of their population, using the `* selector`

    ```css
    [POP_EST < 10000000] {
      fill: lightgrey;
    }

    [POP_EST >= 10000000 AND POP_EST < 50000000] {
      fill: olive;
    }

    [POP_EST > 50000000] {
      fill: salmon
    }

    * {
      stroke: black;
      stroke-width: 0.2;
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137175513-1054570a-66b5-457f-9662-374a29767b6f.png)

## Patterns and Hatches

- Upload the `ESRI Shapefile` named `tl_2010_08013_arealm` from the folder `/opt/data/sample_data/pretty_maps/data/boulder`

    ![image](https://user-images.githubusercontent.com/1278021/137196440-3e9c582e-135f-4eba-a60e-b61b3ed80639.png)

- Create a new `CSS` style named `CSS Area Landmarks` for the `tl_2010_08013_arealm` layer

    ![image](https://user-images.githubusercontent.com/1278021/137196561-a7509c25-887a-438e-b47e-0fb533a84688.png)

- Switch to the `text` editor insert the following `CSS`

    ```css
    [MTFCC='K2582'] [@scale < 500000] {
        fill: url(./img/landmarks/area/grave_yard.png);
        fill-mime: 'image/png';
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137196834-183f458c-210f-4f4b-a96c-e4d72b7234b9.png)

    _The above CSS defines a <PolygonSymbolizer> with a <GraphicFill> pointing to a png ./img/landmarks/area/grave_yard.png in the GeoServer data directory, which will be used by GeoServer as pattern to fill the polygon._

- The previous example uses a `png` as fill pattern; the next one uses a `font character` instead

    ```css
    [MTFCC='K2582'] [@scale < 500000] {
      fill: #D3FFD3, symbol('ttf://Wingdings#0x0055');
      fill-size: 16;
      fill-opacity: 0.5, 1.0;
      stroke: #6DB26D;
      -gt-graphic-margin: 8;
      :nth-symbol(2) {
        stroke: #6DB26D;
      }
    }
    ```
    
    ![image](https://user-images.githubusercontent.com/1278021/137197282-7fd896b9-e4bb-478a-a44a-1c3b59816e29.png)

- Let's now take a look at another way to fill polygons using patterns, the `Hatches`.
- Edit the styles of the GeoNode layer `Wetlands_regulatory_areas`
     
    ![image](https://user-images.githubusercontent.com/1278021/137197567-3b4932c5-41ea-4a52-9c7a-d50027834fdd.png)

- Create a new `CSS` style and name it `CSS Regulatory Area`

    ![image](https://user-images.githubusercontent.com/1278021/137197692-0dd31cef-1260-42fb-83a0-fec4c348415a.png)

- Insert the following `CSS`

    ```css
    [@scale < 10000] {
      fill: symbol('shape://times');
      fill-size: 8;
      :nth-symbol(1) {
        stroke: #ADD8E6;
        stroke-width: 1.0;
      };
    }
    ```

    ![image](https://user-images.githubusercontent.com/1278021/137201075-fb7b7c4e-2e7b-4ca5-bca5-3fda4588962a.png)

    On the previous example we used times as hatches mark. GeoServer makes available different kinds of hatches marks:

    ![image](https://user-images.githubusercontent.com/1278021/137201180-a1886513-7aed-4754-be7e-19d23753788f.png)

#### Dashes

Lets now familiarize a bit with `Dashes`. We are going to see how it's possible to draw several kind of dashes to represent different types of trails or roads.

- Edit the styles of the GeoNode layer `Trails`

    ![image](https://user-images.githubusercontent.com/1278021/137201382-7bdc337a-4a30-4a1b-8254-856f28c4ad5d.png)

- Create a new `CSS` style and name it `CSS Trails`

    ![image](https://user-images.githubusercontent.com/1278021/137201447-86cf7096-da8e-4ee1-84f8-cb4636f47d7f.png)

- Insert the following `CSS`

    ```css
    [@scale < 75000] {
      stroke: #6B4900;
      stroke-width: 0.1;
      stroke-dasharray: 2.0;
    }
    ```

    ![image](https://user-images.githubusercontent.com/1278021/137201623-ac7f133a-a201-4bf1-b85b-4381524dc4a7.png)

- Encodes a dash pattern as a series of numbers separated by spaces. Odd-indexed numbers (first, third, etc) determine the length in pxiels to draw the line, and even-indexed numbers (second, fourth, etc) determine the length in pixels to blank out the line. Default is an unbroken line. Starting from version 2.1 dash arrays can be combined with graphic strokes to generate complex line styles with alternating symbols or a mix of lines and symbols.

- Insert the following `CSS`

    ```css
    [@scale < 75000] {
      stroke: #AA0000, symbol(circle);
      stroke-dasharray: 10 14, 6 18;
      stroke-dashoffset: 14, 0;
      :nth-symbol(2) {
        size: 6;
        fill: #AA0000;
      }
    }
    ```

    ![image](https://user-images.githubusercontent.com/1278021/137201882-ad5853fb-9f79-4d69-94a0-5b12699b7314.png)

    _We may notice two interesting things in this style, two `<LineSymbolizer>` the first one defining a circle `Mark` with a simple `dasharray` and the second one a simple stroke defining also a `dashoffset`. The latter specifies the distance in pixels into the dasharray pattern at which to start drawing. Default is `0`._

#### [Next Section: Publishing Vector Time Series](PUB_VECTOR_TIME_SERIES.md)
