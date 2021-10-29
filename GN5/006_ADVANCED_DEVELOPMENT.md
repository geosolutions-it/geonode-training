# Develop custom branches with geonode-mapstore-client

This section shows how to setup the geonode-mapstore-client to work on custom branch and it should take into account only for advanced/complex changes that previously described customization could not support.

## Setup dev environment

- clone the repository in your workspace

```cmd
git clone --recursive https://github.com/GeoNode/geonode-mapstore-client.git -b 3.2.x
```

- activate virtual environment

```cmd
workon my_geonode
```

- uninstall the current version of `django_geonode_mapstore_client` installed via requirements.txt

```cmd
pip uninstall django_geonode_mapstore_client
```

- navigate inside the geonode-mapstore-client cloned repository

```cmd
cd geonode-mapstore-client/
```

- install the current cloned repository of geonode-mapstore-client as `django_geonode_mapstore_client` module

```cmd
pip install -e .
```

- install all npm module in the client folder of geonode-mapstore-client

```cmd
cd geonode_mapstore_client/client/
```
```cmd
npm install
```

Now the client is configured and all the changes on templates or hooksets should have direct effect to the GeoNode application. If you want to make change on the react component we need additional steps

## Develop the js application of the geonode-mapstore-client repository

- start the development application locally (ensure to be inside the `geonode-mapstore-client/geonode_mapstore_client/client/` folder)

```cmd
npm start
```

- open the url `http://localhost:8081/` to work on the client

Now all the changes made inside the `client/` folder will have effect on all the mapstore application (eg: map viewer, layer viewer)


## Build the client to make it available in static directory

GeoNode uses directly the bundle compiled and committed in the repository so it's important to compile the client and commit it to the repository. We usually follow this approach:
1) make all commits with the changes related to improvements/fixes on the client
2) add an additional commit that contains the results of the `npm run compile` script and refer to previously committed changes (message `update client bundle`).

The `npm run compile` script perform following changes to the repository:

1) it deletes all content of `geonode_mapstore_client/static/mapstore`
2) it creates a version.txt file in the `geonode_mapstore_client/client` directory
3) it creates the bundle of all js and css entries and copy them to the `geonode_mapstore_client/static/mapstore/dist` folder
4) it copies all static contents of `geonode_mapstore_client/client/static/mapstore` to the directory `geonode_mapstore_client/static/mapstore/`
5) it updates the root `package.json`

These is the summary of needed build steps:

- commit all previous changes on the source code
- ensure to be in the `geonode-mapstore-client/geonode_mapstore_client/client/` directory

- run lint script

```cmd
npm run lint
```

- run test

```cmd
npm run test
```

- compile the client

```cmd
npm run compile
```

## Update the requirement.txt of the geonode-project

Now it is possible to install the custom branch inside the geonode-project by updating the requirement.txt to point a specific branch of the django_geonode_mapstore_client, here an example:

```
-e git+https://github.com/{my-fork}/geonode-mapstore-client.git@{commit}#egg=django_geonode_mapstore_client
```
