# Structure of the client: directories and files

The location of files and directories of geonode_mapstore_client module is an important part to because most of the override done by the geonode-project are location based

The structure of the geonode_mapstore_client module is an important part of this documentation because most of the customization done inside geonode-project are overrides based on the location of the files. We have two main group in this module:

- `client` directory contains all source code needed for the MapStore application
    - [Javascript files](#javascript-files)
    - [Themes files](#themes-files)
    - [Configurations files](#configurations-files)
- `static/mapstore` directory contains all the compiled static files used by GeoNode
- `templates` directory contains all the html django templates
    - [HTML templates files](#html-templates-files)

```shell
geonode_mapstore_client/
|-- ...
|-- client/
|    |-- ...
|    |-- js/
|    |-- MapStore2/
|    |-- static/
|    |    +-- mapstore/
|    |         |-- ...
|    |         +-- translations/
|    |-- themes/
|    |    |-- ...
|    |    |-- default/
|    |    +-- preview/
|    |-- ...
|    |-- env.json
|    |-- package.json
|    +-- version.txt
|-- static/
|    |-- ...
|    +-- mapstore/
|    |    |-- ...
|    |    |-- configs/
|    |         +-- localConfig.json
|    |    |-- dist/
|    |         +-- ... 
|    |    |-- extensions/
|    |         +-- index.json
|    |    |-- gn-translations/
|    |    |-- img/
|    |    |-- ms-translations/
|    |    |-- translations/
|    |    +-- version.txt
|-- templates/
|    +-- geonode-mapstore-client/
|-- ...
```

## Javascript files

The `geonode_mapstore_client/client/js/` folder contains all the javascript and jsx files needed to build the application. This folder is targeted by babel loader so it's possible to use javascript es6 features inside .js and .jsx files. This files are compiled with the `npm run compile` script and the bundle is copied in the `static/mapstore/dist directory` to be available from GeoNode templates.

The naming of folder is following the directories and files naming conventions used inside [MapStore](https://mapstore.readthedocs.io/en/latest/developer-guide/plugins-architecture/). The directories are subdivided by function: actions, api, components, epics, hooks, observables, plugins, reducers, routes, utils, ... while the files should be related to a specific plugin name if they are not generic:

eg. The Save plugin will have plugins/Save.jsx, components/save/*.jsx, utils/SaveUtils.jsx, actions/save.js, reducers/save.js, epics/save.js and so on.

Below the structure of the `geonode_mapstore_client/client/js/` folder:

```shell
geonode_mapstore_client/
|-- ...
|-- client/
|    |-- ...
|    +-- js/
|         |-- ...
|         |-- actions/
|         |-- api/
|         |-- apps/
|         |-- components/
|         |-- epics/
|         |-- hooks/
|         |-- observables/
|         |-- plugins/
|         |-- reducers/
|         |-- routes/
|         |-- selector/
|         +-- utils/
|
... -> compiled in
|
|-- static/
|    |-- ...
|    +-- mapstore/
|    |    +-- dist/

```
Some directories and files have special behaviors:

- `geonode_mapstore_client/client/js/apps/`: each file in this folder will be compiled as a new entry point so only .js or .jsx files are allowed. eg. `geonode_mapstore_client/client/js/apps/gn-geostory.js` will become a `gn-geostory.js` file in the dist folder.

## Themes files

The `geonode_mapstore_client/client/themes/` folder contains all the [.less](http://lesscss.org/) files needed to compile the MapStore theme with additional customization. Each theme should be placed inside a folder named as the final expected css file and provide a file `theme.less` as entry point:

eg. `geonode_mapstore_client/client/themes/my-theme/theme.less` will become a `my-theme.css` file in the dist folder.

`geonode-mapstore-client` provides two main style:
- default.css used by the full page map viewer
- preview.css used by the preview map viewer

```shell
geonode_mapstore_client/
|-- ...
|-- client/
|    |-- ...
|    +-- themes/
|         |-- ...
|         |-- default/
|         |    |-- less/
|         |    |-- theme.less
|         |    +-- variables.less
|         +-- preview/
|              |-- less/
|              |-- theme.less
|              +-- variables.less
|
... -> compiled in
|
|-- static/
|    |-- ...
|    +-- mapstore/
|    |    +-- dist/
|    |         |-- default.css
|    |         +-- preview.css
|-- ...
```
The language used for the styles is [less](http://lesscss.org/) and it's compatible with the [MapStore theme](https://mapstore.readthedocs.io/en/latest/developer-guide/customize-theme/).

**Warning**: there is also a new theme called `geonode` but it's a deprecated placeholder that will be removed in new versions of geonode-mapstore-client.

## Configurations files

The MapStore application needs [configurations](https://mapstore.readthedocs.io/en/latest/developer-guide/local-config/) to load the correct plugins or enable/disable/change functionalities.

We need to provide two main type of configuration:
 - apps and plugins configurations is centralized in localConfig.json
 - translations: translations files for the geonode client included in the `geonode_mapstore_client/client/static/translations/` folder

```shell
geonode_mapstore_client/
|-- ...
|-- client/
|    |-- ...
|    |-- static/
|    |    +-- mapstore/
|    |         |-- configs/
|    |         |    |-- ...
|    |         |    |-- localConfig.json
|    |         |-- img/
|    |         +-- translations/
|    |              |-- ...
|    |              |-- data.de-DE.json
|    |              |-- data.en-US.json
|    |              |-- data.es-ES.json
|    |              |-- data.fr-FR.json
|    |              +-- data.it-IT.json
|    |-- ...
|
... -> compiled in
|
|-- static/
|    |-- ...
|    +-- mapstore/
|    |    |-- gn-translations/
|    |    |-- configs/
|    |    |    +-- localConfig.json
|    |    +-- ms-translations/ (from the MapStore2 submodule)
|-- ...
```

**Important!**: The `geonode_mapstore_client/static/mapstore/` is the directory with all the final files generated after running the `npm run compile` script inside the `geonode_mapstore_client/client/` folder. Every new file needed in the `geonode_mapstore_client/static/mapstore/` must be placed inside the `geonode_mapstore_client/client/static/mapstore/` directory then the `npm run compile` will move all the needed files in the final destination including the statics.

## HTML templates files

The HTML templates represents all the pages where the MapStore client is integrated. Each template has its own configuration based on the resource type layer, map or app, and for a specific purpose view, edit or embed.

The _geonode_config.html template is used as base configuration for other templates.

```
geonode_mapstore_client/
|-- ...
|-- templates/
|    +-- geonode-mapstore-client/
|         |-- ...
|         |-- app/
|         |    |-- ...
|         |    +-- geostory.html
|         |-- _geonode_config.html
|         |-- app_edit.html
|         |-- app_embed.html
|         |-- app_list.html
|         |-- app_new.html
|         |-- app_view.html
|         |-- layer_data_edit.html
|         |-- layer_detail.html
|         |-- layer_embed.html
|         |-- layer_style_edit.html
|         |-- layer_view.html
|         |-- map_detail.html
|         |-- map_edit.html
|         |-- map_embed.html
|         |-- map_new.html
|         +-- map_view.html
|-- ...
```
