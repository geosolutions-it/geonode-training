# Add Translations to geonode-project

Django has a native **internationalization** system that allows you to translate your web portal in different languages.

GeoNode comes already with a set of translations (see [here how to contribute to GeoNode core translations](https://docs.geonode.org/en/master/contribute/translation/index.html)).

In order to add translations to the `geonode-project` custom files (`*.py`, `*.html` and `*.js` files), follow the procedure here below

- Create a `locale` folder to store translations if it does not already exist

    ```shell
    mkdir my_geonode/locale
    ```

- Collect `*.py/*.html/*.js/*.txt` translations on different languages (in the example below `en` and `it`)

    ```shell
    django-admin makemessages --no-location -l en -l it -d django -e "html,txt,py" -i docs -i docker
    ```

    ```shell
    django-admin makemessages --no-location -l en -l it -d djangojs -e "js" -i docs -i node_modules -i lib -i docker
    ```

- Review the translations if needed by using an external client like [POEDIT](https://poedit.net/)

    ```shell
    poedit my_geonode/locale/en/LC_MESSAGES/django.po
    ```

- Compile the messages

    ```shell
    django-admin compilemessages
    ```

## Push Changes to GitHub

```shell
git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   my_geonode/settings.py
	modified:   my_geonode/urls.py

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	docker/geoserver/requirements.txt.py
	geocollections/
	my_geonode/locale/
	my_geonode/templates/geocollections/
	my_geonode/urls.py.org
```

```shell
git add -A
```

```shell
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   docker/geoserver/requirements.txt.py
	new file:   geocollections/__init__.py
	new file:   geocollections/admin.py
	new file:   geocollections/api.py
	new file:   geocollections/api.py.org
	new file:   geocollections/apps.py
	new file:   geocollections/migrations/0001_initial.py
	new file:   geocollections/migrations/0002_auto_20210913_1738.py
	new file:   geocollections/migrations/__init__.py
	new file:   geocollections/models.py
	new file:   geocollections/models.py.org
	new file:   geocollections/models.py.org_1
	new file:   geocollections/permissions.py
	new file:   geocollections/serializers.py
	new file:   geocollections/serializers.py.org
	new file:   geocollections/tests.py
	new file:   geocollections/urls.py
	new file:   geocollections/urls.py.org
	new file:   geocollections/views.py
	new file:   geocollections/views.py.org
	new file:   geocollections/views.py.org_1
	new file:   geocollections/views.py.org_2
	new file:   my_geonode/locale/en/LC_MESSAGES/django.mo
	new file:   my_geonode/locale/en/LC_MESSAGES/django.po
	new file:   my_geonode/locale/it/LC_MESSAGES/django.mo
	new file:   my_geonode/locale/it/LC_MESSAGES/django.po
	modified:   my_geonode/settings.py
	new file:   my_geonode/templates/geocollections/geocollection_detail.html
	new file:   my_geonode/templates/geocollections/geocollection_permissions.html
	modified:   my_geonode/urls.py
	new file:   my_geonode/urls.py.org
```

```shell
git commit -a -m " - GeoCollections App + Translations"
```

```shell
git push <fork name> <branch name>    <-- e.g.: git push afabiani main
```

#### [Next Section: Put geonode-project in Production](090_GEONODE_PROJ_PROD.md)
