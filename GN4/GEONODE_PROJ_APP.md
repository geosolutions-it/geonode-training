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

    def __unicode__(self):
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


### [Next Section: Add Tanslations to geonode-project](GEONODE_PROJ_TRX.md)
