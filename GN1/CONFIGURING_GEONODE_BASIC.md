# Introduction to Configuring GeoNode (Basics)
GeoNode is built on top of Django. As a Django application GeoNode follows the common rules of a Django project.

In particular every Django project must have a special python file called `settings.py` which will be used to set the global variables and behavior of the whole framework.

Some `settings` variables are mandatory for a Django project, as an instance the `version`, the `site url`, the `allowed hosts`, the `database configuration`, the `debug mode`, etc... For more information about the common Django `settings` you can check the official [Django documentation](https://docs.djangoproject.com/en/3.2/topics/settings/). 

## GeoNode Settings
GeoNode, of course, has this file too which is located into the `/opt/geonode/geonode/settings.py` location.

 - In order to inspect the contents of the GeoNode `settings` file, open a file system browser and open it with a text editor

    ![image](https://user-images.githubusercontent.com/1278021/125773982-ec16f915-eaf8-4341-8591-d2bff95d2000.png)

There are plenty of variables listed on the `settings` file, most of them have a notation similar to the following one:

   ```python
   SITEURL = os.getenv('SITEURL', _default_siteurl)
   ```

That means that GeoNode looks first for the variable value into the `System Environment` and, if no value or variable has been found, it falls back to a `default` one.


#### [Next Section: Introduction to Administering GeoNode (Basics)](ADMINISTERING_GEONODE_BASIC.md)
