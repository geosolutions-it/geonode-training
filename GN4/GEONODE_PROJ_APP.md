# Add an App with APIs to geonode-project
In this section, we will show how to create and setup the skeleton of a `custom app` using the django facilities.

The **Geocollections app** will show in a single page, `resources` and `users` grouped by a `GeoNode Profile`.

We will be able to assign arbitrary `resources`, a `profile` and a `name` to a `Geocollection`; the latter will be used to build a `dedicated URL` too.

## Create the `django app`
- Django provides us handy commands to create and manage `apps`. We already used `startproject` to create our `geonode-project`, now we will use `startapp` to create an `app`.

```shell
cd /opt/geonode-project/my_geonode
./manage_dev.sh startapp geocollections
```

- This will create a folder named `geocollections` that contains empty `models` and `views`.
- We need to add the new app to the `INSTALLED_APPS` of our project. Edit `my_geonode/settings.py` and go to `line 60`:

```shell
vim my_geonode/settings.py
```

```diff
diff --git a/my_geonode/settings.py b/my_geonode/settings.py
index d9ac76a..786b29f 100644
--- a/my_geonode/settings.py
+++ b/my_geonode/settings.py
@@ -57,7 +57,7 @@ WSGI_APPLICATION = "{}.wsgi.application".format(PROJECT_NAME)
 LANGUAGE_CODE = os.getenv('LANGUAGE_CODE', "en")
 
 if PROJECT_NAME not in INSTALLED_APPS:
-    INSTALLED_APPS += (PROJECT_NAME, )
+    INSTALLED_APPS += (PROJECT_NAME, 'geocollections', )
 
 # Location of url mappings
 ROOT_URLCONF = os.getenv('ROOT_URLCONF', '{}.urls'.format(PROJECT_NAME))
```
 
## Add Custom Models, Views and URLs
```shell
vim geocollections/models.py
```
```python
from django.db import models

from geonode.base.models import ResourceBase
from geonode.groups.models import GroupProfile


class Geocollection(models.Model):
    """
    A collection is a set of resources linked to a GeoNode group
    """
    group = models.ForeignKey(GroupProfile, related_name='group_collections', on_delete=models.CASCADE)
    resources = models.ManyToManyField(ResourceBase, related_name='resource_collections')
    name = models.CharField(max_length=128, unique=True)
    slug = models.SlugField(max_length=128, unique=True)

    def __str__(self):
        return self.name
```

- At this point we need to ask django to create the database table. Django since version 1.8 has embedded migrations mechanism and we need to use them in order to change the state of the db.

```shell
./manage_dev.sh makemigrations

# the command above shows the migrations to be executed on the database
Migrations for 'geocollections':
  geocollections/migrations/0001_initial.py
    - Create model Geocollection

./manage_dev.sh migrate

Operations to perform:
  Apply all migrations: account, actstream, admin, announcements, auth, avatar, base, br, contenttypes, dialogos, django_celery_beat, django_celery_results, documents, favorite, geoapp_geostories, geoapps, geocollections, geonode_client, geonode_themes, groups, guardian, invitations, layers, maps, mapstore2_adapter, monitoring, oauth2_provider, people, pinax_notifications, ratings, services, sessions, sites, socialaccount, taggit, tastypie, upload, user_messages
Running migrations:
  Applying geocollections.0001_initial... OK
```

- Let's add a `django generic view` to show the collections detail.

```shell
vim geocollections/views.py
```
```python
from django.views.generic import DetailView

from .models import Geocollection

class GeocollectionDetail(DetailView):
    model = Geocollection
```

- In order to access the `view` we just created, we will need some `url mapping` definitions. The `urls.py` file contains a `url mapping` to our `generic view`

```shell
vim geocollections/urls.py
```
```python
from django.conf.urls import url

from .views import GeocollectionDetail

urlpatterns = [
    url(r'^(?P<slug>[-\w]+)/$',
        GeocollectionDetail.as_view(),
        name='geocollection-detail'),
]
```

- We also need to register the `app urls` to the `geonode-project urls`.
- Let's modify the `my_geonode` `urls.py` file adding the following mappings

```shell
vim my_geonode/urls.py
```
```diff
diff --git a/my_geonode/urls.py b/my_geonode/urls.py
index 07b694f..bcf1cb7 100644
--- a/my_geonode/urls.py
+++ b/my_geonode/urls.py
@@ -26,7 +26,7 @@ from geonode.base import register_url_event
 
 urlpatterns += [
 ## include your urls here
-
+    url(r'^geocollections/', include('geocollections.urls')),
 ]
 
 homepage = register_url_event()(TemplateView.as_view(template_name='site_index.html'))
```

## Enable the `geocollections` App into the Admin Panel
- We need a user interface allowing us to create `geocollections`.
- Django makes this very easy, we just need adding them to the `admin.py` file as follows

```shell
vim geocollections/admin.py
```
```python
from django.contrib import admin

from .models import Geocollection


class GeocollectionAdmin(admin.ModelAdmin):
    prepopulated_fields = {"slug": ("name",)}
    filter_horizontal = ('resources',)

admin.site.register(Geocollection, GeocollectionAdmin)
```

- Move to `http://localhost:8000/admin/` and search for the `Geocollections tab`


    ![image](https://user-images.githubusercontent.com/1278021/133110817-1c17667f-abce-44e3-bd34-89f1301c3993.png)

- Create a new `Geocollection` named `boulder` and add some `resources` to it

    ![image](https://user-images.githubusercontent.com/1278021/133111090-4ee7e3fc-e7fe-4ecd-b6fb-26cc7adc9d2a.png)

## Adding the Geocollections Details Template
- Last thing we need to add in order to render the Geocollection details, is the `HTML template` used by the `django view`

```shell
mkdir -p my_geonode/templates/geocollections/
```
```shell
vim my_geonode/templates/geocollections/geocollection_detail.html
```
```django
{% extends "geonode_base.html" %}
{% block body %}
    <h2>Geocollection {{ object.name }}</h2>
    <p>Group: {{ object.group.title }}</p>
    <p>Resources:</p>
    <ul>
        {% for resource in object.resources.all %}
            <li>{{ resource.title }}</li>
        {% endfor %}
    </ul>
{% endblock %}
```

- Try now visiting the `geocollection` we just created; go to `http://localhost:8000/geocollections/boulder/`
- The `URLs` of the `geocollection` are in the form `http://localhost:8000/geocollections/<the-name-of-the-created-geocollection>`

    ![image](https://user-images.githubusercontent.com/1278021/133127678-5594625a-7aa3-4c62-a4a0-8948bbd3abe8.png)

## Permissions
The permissions in GeoNode are managed by `django-guardian`, a python library allowing to set _object level permissions_ (django has table level authorization).

- First thing to do is to add the `permissions object` to the `database`. We can do this by adding the following `meta class` to our `Geocollection model`, `guardian` will take care of creating the objects for us.

```shell
vim geocollections/models.py
```
```diff
--- geocollections/models.py.org	2021-09-13 18:36:24.181330710 +0100
+++ geocollections/models.py	2021-09-13 18:35:29.788540726 +0100
@@ -16,3 +16,7 @@
     def __str__(self):
         return self.name
 
+    class Meta:
+        permissions = (
+            ('access_geocollection', 'Can view geocollection'),
+        )
```

- Run `makemigrations` and `migrate` managemnt commands to install them

```shell
./manage_dev.sh makemigrations
./manage_dev.sh migrate
```

- Notice that it is not possible define some `permissions` anymore with prefixes like `view_`, `add_`, `delete_` or something, because those have been natively introuced by Django since version 2.1
 
### [Next Section: Add Tanslations to geonode-project](GEONODE_PROJ_TRX.md)
