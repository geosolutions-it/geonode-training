# Add a new plugin extension

This section shows how to add a plugin using the extension support in MapStore without re-compile of the bundle of the geonode-mapstore-cient applications. This new extensibility should be used for simple plugins.

Note: the version of mapstore used in this extension is different from the version used by geonode-mapstore-client 3.2.x so some functionalities of the framework could not be available

## Setup extension

- navigate to the `my_geonode/` directory

```cmd
cd my_geonode/
```

- run the create script using the mapstore project

```cmd
npx @mapstore/project create extension
```

- these are the properties requested by the create script. MyGeoNodeExtension is the name of the extension and it should be a unique identifier

```cmd
- Name of extension (default Extension): MyGeoNodeExtension
- Name of project (default mapstore-extension): client
- Run npm install after creation setup (yes/no default yes): no
```

- create an index.json file inside the `my_geonode/static/mapstore/extensions` directory

```cmd
my_geonode/
|-- ...
|-- my_geonode/
|    |-- ...
|    |-- client/
|    +-- static/
|         |-- ...
|         +-- mapstore/
|              |-- ...
|              +-- extensions/
|                   |-- ...
|                   +-- index.json
|-- ...
```

- configure the `extensions/index.json` file to include the new extension

```json
{
    "MyGeoNodeExtension": {
        "bundle": "/static/mapstore/extensions/MyGeoNodeExtension/index.js",
        "translations": "/static/mapstore/extensions/MyGeoNodeExtension/translations",
        "assets": "/static/mapstore/extensions/MyGeoNodeExtension/assets"
    }
}
```

- navigate to the client folder

```cmd
cd client/
```

- add license file if missing LICENSE.txt
- add the `postCompile.js` script file to move the compiled bundle in the correct directory

```js
const fs = require('fs-extra');
const path = require('path');
const extensionsPath = path.join(__dirname, '..', 'static', 'mapstore', 'extensions');
const extensionIndexPath = path.join(extensionsPath, 'index.json');
const extensionIndex = fs.existsSync(extensionIndexPath) ? require(extensionIndexPath) : {};

const EXTENSION_NAME = 'MyGeoNodeExtension';

let error = false;
Object.keys(extensionIndex).forEach(extensionName => {
    const endpoints = extensionIndex[extensionName];
    Object.keys(endpoints).forEach(key => {
        if (endpoints[key].match('http://localhost:8082/extension')) {
            error = true;
            console.error('');
            console.error('//////////////////////////////////////');
            console.error('/// Error: dev url ' + key + ': ' + endpoints[key]);
            console.error('/// -> please replace http://localhost:8082/extension with /static/mapstore/extensions/' + extensionName);
            console.error('//////////////////////////////////////');
            console.error('');
        }
    });
});

if (!error) {
    fs.moveSync(path.resolve(__dirname, 'dist'), path.resolve(extensionsPath, EXTENSION_NAME), { overwrite: true });
}
```

- modify the package.json to use the new postCompile.js script and to target the mapstore version 2021.02.xx.

```diff
"scripts": {
-   "compile": "mapstore-project compile extension",
+   "compile": "mapstore-project compile extension && node ./postCompile.js",
    "lint": "eslint js --ext .jsx,.js",
    "start": "mapstore-project start extension",
    "test": "mapstore-project test extension",
    "test:watch": "mapstore-project test:watch extension"
},
...
"dependencies": {
-   "mapstore": "git+https://github.com/geosolutions-it/MapStore2.git#master"
+   "mapstore": "git+https://github.com/geosolutions-it/MapStore2.git#2021.02.xx"
},
```

- install all needed dependencies

```cmd
npm install
```

- compile the bundle to verify if all the scripts are configured correctly

```cmd
npm run compile
```

- after compile a new folder is generated inside the extensions directory

```cmd
my_geonode/
|-- ...
|-- my_geonode/
|    |-- ...
|    +-- static/
|         |-- ...
|         +-- mapstore/
|              |-- ...
|              +-- extensions/
|                   |-- MyGeoNodeExtension/
|                   +-- index.json
|-- ...
```

- edit the `my_geonode/templates/geonode-mapstore-client/_geonode_config.html` template to include this new extension

```django
<!-- _geonode_config.html file in the my_geonode project -->
{% extends 'geonode-mapstore-client/_geonode_config.html' %}
{% block override_local_config %}
<script>
    window.__GEONODE_CONFIG__.overrideLocalConfig = function(localConfig) {
        // map_edit page used for path /maps/{pk}/edit
        localConfig.plugins.map_edit.push({ "name": "MyGeoNodeExtension" });
        // map_view page used for path /maps/{pk}/view
        localConfig.plugins.map_view.push({ "name": "MyGeoNodeExtension" });
        return localConfig;
    };
</script>
{% endblock %}
```

After this steps a box with a message is visible on the map viewer

## Develop the extension

- temporary edit the `my_geonode/static/mapstore/extensions/index.json` file using the dev endpoints instead of the static ones (remember to restore this value to the static one)

```json
{
    "MyGeoNodeExtension": {
        "bundle": "http://localhost:8082/extension/index.js",
        "assets": "http://localhost:8082/extension/assets",
        "translations": "http://localhost:8082/translations"
    }
}
```

**Warning** If there are CORS issue restore the translations path to the static one (/static/mapstore/extensions/MyGeoNodeExtension/translations).

```json
{
    "MyGeoNodeExtension": {
        "bundle": "http://localhost:8082/extension/index.js",
        "assets": "http://localhost:8082/extension/assets",
        "translations": "/static/mapstore/extensions/MyGeoNodeExtension/translations"
    }
}
```

- start the dev endpoints

```cmd
npm start
```

- replace the content of the `my_geonode/client/js/extension/plugins/Extension.jsx` with a new extension. An example could be a floating box that shows the value of the center of the map

```jsx
import React from 'react';
import PropTypes from 'prop-types';
import { connect } from 'react-redux';
import { createSelector } from 'reselect';
import { mapSelector } from '@mapstore/framework/selectors/map';

function MyGeoNodeExtension({
    center
}) {
    return (
        <div
            className="shadow"
            style={{
                position: 'absolute',
                zIndex: 100,
                bottom: 35,
                margin: 8,
                left: '50%',
                backgroundColor: '#ffffff',
                padding: 8,
                textAlign: 'center',
                transform: 'translateX(-50%)'
            }}
        >
            <div>
                <small>
                    Map Center ({center.crs})
                </small>
            </div>
            <div>
                x: <strong>{center?.x?.toFixed(6)}</strong>
                {' | '}
                y: <strong>{center?.y?.toFixed(6)}</strong>
            </div>
        </div>
    );
}

MyGeoNodeExtension.propTypes = {
    center: PropTypes.object
};

MyGeoNodeExtension.defaultProps = {
    center: {}
};

const MyGeoNodeExtensionPlugin = connect(
    createSelector([
        mapSelector
    ], (map) => ({
        center: map?.center
    })),
    {}
)(MyGeoNodeExtension);

export default {
    name: __MAPSTORE_EXTENSION_CONFIG__.name,
    component: MyGeoNodeExtensionPlugin,
    containers: {},
    epics: {},
    reducers: {}
};
```

## Build the extension

- restore the `extension/index.json` points to `/static/mapstore/extensions/MyGeoNodeExtension` path for MyGeoNodeExtension extension

```json
{
    "MyGeoNodeExtension": {
        "bundle": "/static/mapstore/extensions/MyGeoNodeExtension/index.js",
        "assets": "/static/mapstore/extensions/MyGeoNodeExtension/assets",
        "translations": "/static/mapstore/extensions/MyGeoNodeExtension/translations"
    }
}
```

- compile the bundle

```cmd
npm run compile
```
