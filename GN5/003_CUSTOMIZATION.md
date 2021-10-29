# Customization of html templates and configuration files

The localConfig.json is the main configuration files for MapStore and can be used to change pages structure by including, updating or removing plugins.
The geonode-mapstore-project expose a global function called `overrideLocalConfig` that allows overrides in a geonode-project. These are the steps to setup the localConfig override:

1) create a new directory named `geonode-mapstore-client` inside the `my_geonode/my_geonode/templates/` directory

2) create a new html template named `_geonode_config.html` inside the `my_geonode/my_geonode/templates/geonode-mapstore-client/` directory

```
my_geonode/
|-- ...
|-- my_geonode/
|    |-- ...
|    +-- templates/
|         |-- ...
|         +-- geonode-mapstore-client/
|              +-- _geonode_config.html
|-- ...
```

3) add the following block extension in the `_geonode_config.html` template 

{% raw %}
```django
<!-- _geonode_config.html file in the my_geonode project -->
{% extends 'geonode-mapstore-client/_geonode_config.html' %}
{% block override_local_config %}
<script>
    window.__GEONODE_CONFIG__.overrideLocalConfig = function(localConfig) {
        // this function must return always a valid localConfig json object
        return localConfig;
    };
</script>
{% endblock %}
```
{% endraw %}

Now the `window.__GEONODE_CONFIG__.overrideLocalConfig` function can be used to override the localConfig json file.

## How to use the overrideLocalConfig function

- Override plugin configuration in a page (plugin configuration available here https://mapstore.geosolutionsgroup.com/mapstore/docs/api/plugins)

Note: not all configuration can be applied to the geonode-mapstore-client because the environment is different from the MapStore product

{% raw %}
```django
<!-- _geonode_config.html file in the my_geonode project -->
{% extends 'geonode-mapstore-client/_geonode_config.html' %}
{% block override_local_config %}
<script>
    window.__GEONODE_CONFIG__.overrideLocalConfig = function(localConfig) {
        // an example on how you can apply configuration to existing plugins
        // example: DrawerMenu width (left panel)
        var selectedPluginName = "DrawerMenu";
        var pluginPageName = "map_edit";

        var TOC_WIDTH = 400;

        // ensure the map layout has the correct size to move the background selector
        // when the layer tree is open
        localConfig.mapLayout.viewer.left.sm = TOC_WIDTH;

        for (var i = 0; i < localConfig.plugins[pluginPageName].length; i++) {
            var currentPlugin = localConfig.plugins[pluginPageName][i];
            var isSelectedPlugin = currentPlugin.name === selectedPluginName;
            if (isSelectedPlugin) {
                // apply configuration to the plugin
                localConfig.plugins[pluginPageName][i] = {
                    "name": selectedPluginName,
                    "cfg": {
                        "menuOptions": {
                            "width": TOC_WIDTH
                        }
                    }
                }
            }
        }
        return localConfig;
    };
</script>
{% endblock %}
```
{% endraw %}

- Restore a plugin in a page

{% raw %}
```django
<!-- _geonode_config.html file in the my_geonode project -->
{% extends 'geonode-mapstore-client/_geonode_config.html' %}
{% block override_local_config %}
<script>
    window.__GEONODE_CONFIG__.overrideLocalConfig = function(localConfig) {
        /*
        "SearchServicesConfig" has been disabled by default but still available
        inside the list of imported plugin.
        It should be enabled only in the pages that contains the "Search" plugin.
        */
        // map_edit page used for path /maps/{pk}/edit
        localConfig.plugins.map_edit.push({ "name": "SearchServicesConfig" });
        // map_view page used for path /maps/{pk}/view
        localConfig.plugins.map_view.push({ "name": "SearchServicesConfig" });

        return localConfig;
    };
</script>
{% endblock %}
```
{% endraw %}

- Remove a plugin from a page

{% raw %}
```django
{% extends 'geonode-mapstore-client/_geonode_config.html' %}
{% block override_local_config %}
<script>
    window.__GEONODE_CONFIG__.overrideLocalConfig = function(localConfig) {
        // an example on how you can remove a plugin from configuration
        // example: Measure
        var removePluginName = "Measure";
        var pluginPageName = "map_edit";
        var newMapPlugins = [];
        for (var i = 0; i < localConfig.plugins[pluginPageName].length; i++) {
            var currentPlugin = localConfig.plugins[pluginPageName][i];
            var isRemovePlugin = currentPlugin.name === removePluginName;
            if (!isRemovePlugin) {
                newMapPlugins.push(currentPlugin);
            }
        }
        // map_edit page used for path /maps/{pk}/edit
        localConfig.plugins[pluginPageName] = newMapPlugins;
        return localConfig;
    };
</script>
{% endblock %}
```
{% endraw %}

## Customize default map env variables

There are some env variables that allows to override the default map configuration used in the maps/new path

```cmd
DEFAULT_MAP_CENTER_X = 0 # initial x center position of the map (CRS84)
DEFAULT_MAP_CENTER_Y = -30 # initial y center position of the map (CRS84)
DEFAULT_MAP_ZOOM = 3 # initial zoom level of the map
```

you can add also the list of default background layers with the `MAPSTORE_BASELAYERS` variable inside the settings.py:

```py
MAPSTORE_BASELAYERS = [
    {
        "type": "osm",
        "title": "Open Street Map",
        "name": "mapnik",
        "source": "osm",
        "group": "background",
        "visibility": True
    },
    {
        "source": "ol",
        "group": "background",
        "id": "none",
        "name": "empty",
        "title": "Empty Background",
        "type": "empty",
        "visibility": False,
        "args": ["Empty Background", {"visibility": False}]
    },
    {
        "format": "image/jpeg",
        "group": "background",
        "name": "osm:osm_simple_dark",
        "opacity": 1,
        "title": "OSM Simple Dark",
        # "thumbURL": "path/to/thumb/image",
        "type": "wms",
        "url": [
            "https://maps6.geosolutionsgroup.com/geoserver/wms",
            "https://maps3.geosolutionsgroup.com/geoserver/wms",
            "https://maps1.geosolutionsgroup.com/geoserver/wms",
            "https://maps4.geosolutionsgroup.com/geoserver/wms",
            "https://maps2.geosolutionsgroup.com/geoserver/wms",
            "https://maps5.geosolutionsgroup.com/geoserver/wms"
        ],
        "source": "osm_simple_dark",
        "visibility": False,
        "singleTile": False,
        "credits": {
            "title": "OSM Simple Dark | Rendering <a href=\"https://www.geo-solutions.it/\">GeoSolutions</a> | Data © <a href=\"http://www.openstreetmap.org/\">OpenStreetMap</a> contributors, <a href=\"http://www.openstreetmap.org/copyright\">ODbL</a>"
        }
    }
]
```

here you can find documentation related to layer types supported by mapstore

- https://mapstore.readthedocs.io/en/latest/developer-guide/maps-configuration/#layer-types

Note: WMTS layers are not currently supported as background layer

here the default background layers configuration used by geonode

- https://github.com/GeoNode/geonode/blob/3.2.x/geonode/settings.py#L1553-L1617
