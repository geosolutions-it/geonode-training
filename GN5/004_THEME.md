# Theme style overrides

This section shows how to apply theme overrides to the MapStore theme (eg: change button primary color)

## Setup the theme dependencies

- navigate to the `my_geonode/my_geonode/static/` directory and edit the `package.json` file to include scripts and dependencies

```diff
{
  "name": "my_geonode",
  "version": "0.0.1",
  "author": "GeoNode Developers",
  "description": "Static code and assets for my_geonode",
  "contributors": [],
  "scripts": {
    "test": "jshint **.js",
+   "compile-less": "lessc -x ./less/site_base.less ./css/site_base.css",
+   "watch-less": "less-watch-compiler ./less/ ./css/ site_base.less"
  },
  "license": "BSD",
  "private": "false",
  "dependencies": {
+   "bootstrap": "3.4.1"
  },
  "devDependencies": {
    "del": "*",
    "gulp": "^3.9.0",
    "gulp-concat": "*",
    "gulp-less": "*",
    "gulp-util": "*",
+   "less": "4.1.2",
+   "less-watch-compiler": "1.16.3",
    "path": "*"
  }
}
```

- install npm dependencies inside the `static/` folder

```cmd
npm install
```

- verify that all the style previously added only in the `my_geonode/my_geonode/static/less/site_base.css` are also copied in the `my_geonode/my_geonode/static/less/site_base.less` file

## Customize the theme

- run the watch command to listen to all the changes on the less file

```cmd
npm run watch-less
```

- add to the `my_geonode/my_geonode/static/less/site_base.less` file the following code to change the colors of primary and success buttons

```less
// import the utils/mixins from bootstrap without compile the whole style
@import (reference) "../node_modules/bootstrap/less/bootstrap.less";

// all the style for mapstore client should be wrapped inside the .msgapi class
.msgapi {
    // use the .button-variant function from bootstrap to apply the correct style to the primary and success style
    .btn-primary {
        .button-variant(#1b1b1b; #ffa632; #ac6f1f;);
    }
    .btn-success {
        .button-variant(#ffffff; #2ea372; #0f774c;);
    }
    // add other overrides if needed
}
```
- refresh the browser and verify that the buttons in the viewer has changed color

## Compile the final css file

Once we are ready with the customizations of the theme we can run the compile script to get the minified css file (`my_geonode/my_geonode/static/less/site_base.css`)

```cmd
npm run compile-less
```

**Warning**: the compile and watch scripts will replace all the content of the `css` file with the one compiled from the `less` file.
