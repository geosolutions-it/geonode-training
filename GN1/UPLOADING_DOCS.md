# Adding Other Media Types Contents
As a content management system, GeoNode allows also uploading datasets which are not strictly *georeferences*, i.e. media contents that do not have any sort of geospatial information, like the layers.

GeoNode currently supports a bunch of different media types, like documents, PDFs, images, videos and also audios.

In order to get the whole list of the media type supported, from the `Data` dropdown menu of the `navbar`, select `Upload Document`
   ![image](https://user-images.githubusercontent.com/1278021/125596725-0f3df4c1-2bd4-49d5-8ede-45cb847b51c8.png)

You will be redirected to a page allowing to add a new *document* to GeoNode
   ![image](https://user-images.githubusercontent.com/1278021/125596918-fac5a8fd-3f5f-4166-91e5-07e9321b99d1.png)

- At the top of the page you can notice the list of supported extensions (i.e. media types).
- There are two types of documents that you can create on GeoNode:
   1. **Local documents**; this is content that will be uploaded from your PC directly to the platform
   2. **Remote documents**; those are URLs referencing to external resources. Notice that those files won't be copied locally. GeoNode will just create a reference to them

## Uploading some Local Documents
 - Through the `Upload Document` page, click on `Choose file`; that will open a system file browser. Go to the `/opt/data/sample_data/documents` location
     ![image](https://user-images.githubusercontent.com/1278021/125597787-afbd5ad6-03f7-4b7b-8a83-c18716af50c2.png)

 - Select the file `images/flowers.jpg` and click `Open`; notice that GeoNode will automatically set the document `title` equal to the `file name`
 
   **NOTE**: You can change the title before uploading it if you need; it can be changed on a later stage too if needed
  
     ![image](https://user-images.githubusercontent.com/1278021/125598146-adceb591-81a5-446f-a53e-0be630105262.png)

 - Click on `Upload` and wait for GeoNode to finalize the procedure. You will be automatically redirected to the document details page
     ![image](https://user-images.githubusercontent.com/1278021/125598687-dfdc225e-5f07-4be6-bf26-e6b37e4b5250.png)

Notice that GeoNode tries to create a preview of the media content if possible.

Similarly it tries to create a nice thumbnail if able to read the content-type. It will use a sample image otherwise.

   ![image](https://user-images.githubusercontent.com/1278021/125599966-657f67d0-8296-408f-8a18-05c795e76bb4.png)

### EXIF Metadata for Image Contents
Exchangeable image file format (officially `Exif`, according to `JEIDA/JEITA/CIPA` specifications) is a standard that specifies the formats for images, sound, and ancillary tags used by digital cameras (including smartphones), scanners and other systems handling image and sound files recorded by digital cameras.

It allows storing some metadata info withing the image or audio format.

GeoNode tries to read some of those metadata fields and use them to fill the content metadata.

As an instance, if you go back to the `flowers.jpg` details page, you will be able to notice that GeoNode automatically filled the `abstract` and `keywords

#### [Next Section: Playing with GeoStories](ADD_GEOSTORIES.md)
