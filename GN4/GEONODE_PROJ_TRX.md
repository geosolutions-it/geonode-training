# Add Translations to geonode-project
Django has native **internationalization** system that allows you to translate your web portal in several languages.

GeoNode comes already with a set of translations (see [here how to contribute to GeoNode core translations](https://docs.geonode.org/en/master/contribute/translation/index.html)).

In order to add translations to the `geonode-project` custom files, bot `*.py` and `*.html/*.js`, follow the procedure here below

- Create a `locale` folder to store translations if not exists already

```shell
mkdir my_geonode/locale
```

- Collect `*.py/*.html/*.txt` translations on different languages (in the example below `en` and `it`)

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

#### [Next Section: Put geonode-project in Production](GEONODE_PROJ_PROD.md)
