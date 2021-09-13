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

- Notice that it is not possible to define some `permissions` anymore with prefixes like `view_`, `add_`, `delete_` or something, because those have been natively introuced by Django since version 2.1

### Permission Logic Methods
- Let's add few methods to the `Geocollection` `models` and `views` in order to be able to manage the `permissions`

#### Default Permissions
```shell
vim geocollections/models.py
```
```diff
--- geocollections/models.py.org	2021-09-13 18:36:24.181330710 +0100
+++ geocollections/models.py	2021-09-13 21:47:33.046309700 +0100
@@ -1,4 +1,11 @@
 from django.db import models
+from django.conf import settings
+from django.contrib.auth.models import Group
+from django.contrib.auth import get_user_model
+from django.contrib.contenttypes.models import ContentType
+
+from guardian.shortcuts import assign_perm
+from guardian.models import UserObjectPermission, GroupObjectPermission
 
 from geonode.base.models import ResourceBase
 from geonode.groups.models import GroupProfile
@@ -13,6 +20,34 @@
     name = models.CharField(max_length=128, unique=True)
     slug = models.SlugField(max_length=128, unique=True)
 
+    def remove_object_permissions(self):
+        UserObjectPermission.objects.filter(content_type=ContentType.objects.get_for_model(self),
+                                            object_pk=self.id).delete()
+        GroupObjectPermission.objects.filter(content_type=ContentType.objects.get_for_model(self),
+                                             object_pk=self.id).delete()
+
+    def set_default_permissions(self):
+        """
+        Set default permissions.
+        """
+        # remove all permissions
+        self.remove_object_permissions()
+
+        # default permissions for anonymous users
+        anonymous_group, created = Group.objects.get_or_create(name='anonymous')
+
+        # assign permissions to 'Anyone' if 'DEFAULT_ANONYMOUS_VIEW_PERMISSION' is True
+        if settings.DEFAULT_ANONYMOUS_VIEW_PERMISSION:
+            assign_perm('access_geocollection', anonymous_group, self)
+            
+
+        # default permissions to the Geocollection group members
+        assign_perm('access_geocollection', self.group.group, self)
+
     def __str__(self):
         return self.name
 
+    class Meta:
+        permissions = (
+            ('access_geocollection', 'Can view geocollection'),
+        )
```

- Let's test the `set_default_permissions`

```shell
./manage_dev.sh shell
```
```python
Python 3.8.10 (default, Jun  2 2021, 10:49:15) 
Type 'copyright', 'credits' or 'license' for more information
IPython 7.24.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from geocollections.models import Geocollection

In [2]: Geocollection.objects.first().set_default_permissions()

In [3]: quit()
```

#### Permissions Setter on `perm_spec`
- A `perm_spec` in GeoNode is a `JSON` object declaring the set of `permissions` to assign to `users`, `groups` or both of them.
- Be careful, with `group` here we mean a `Django authority group` which is relared to a GeoNode `GroupProfile` throught its `slug`.
- A sample `perm_spec` is something like the one here below:
    ```json
    perm_spec = {
        "users": {
            "AnonymousUser": [],
            "test_user1": ["access_geocollection"],
            "test_user2": [],
        },
        "groups": {
            "registered-members": ["access_geocollection"]
        }
    }
    ```

```shell
vim geocollections/models.py
```
```diff
--- geocollections/models.py.org	2021-09-13 18:36:24.181330710 +0100
+++ geocollections/models.py	2021-09-13 22:54:31.946056180 +0100
@@ -1,8 +1,22 @@
+import logging
+import traceback
+
 from django.db import models
+from django.conf import settings
+from django.contrib.auth.models import Group, Permission
+from django.contrib.auth import get_user_model
+from django.core.exceptions import ObjectDoesNotExist
+from django.contrib.contenttypes.models import ContentType
+
+from guardian.utils import get_user_obj_perms_model
+from guardian.shortcuts import assign_perm, get_groups_with_perms
+from guardian.models import UserObjectPermission, GroupObjectPermission
 
 from geonode.base.models import ResourceBase
 from geonode.groups.models import GroupProfile
 
+logger = logging.getLogger(__name__)
+
 
 class Geocollection(models.Model):
     """
@@ -13,6 +27,130 @@
     name = models.CharField(max_length=128, unique=True)
     slug = models.SlugField(max_length=128, unique=True)
 
+    def get_users_with_perms(self):
+        """
+        Override of the Guardian get_users_with_perms
+        """
+        ctype = ContentType.objects.get_for_model(self)
+        permissions = {}
+
+        for perm in Permission.objects.filter(codename='access_geocollection', content_type_id=ctype.id):
+            permissions[perm.id] = perm.codename
+
+        user_model = get_user_obj_perms_model(self)
+        users_with_perms = user_model.objects.filter(object_pk=self.pk,
+                                                     content_type_id=ctype.id,
+                                                     permission_id__in=permissions).values('user_id', 'permission_id')
+
+        users = {}
+        for item in users_with_perms:
+            if item['user_id'] in users:
+                users[item['user_id']].append(permissions[item['permission_id']])
+            else:
+                users[item['user_id']] = [permissions[item['permission_id']], ]
+
+        profiles = {}
+        for profile in get_user_model().objects.filter(id__in=list(users.keys())):
+            profiles[profile] = users[profile.id]
+
+        return profiles
+
+    def remove_object_permissions(self):
+        UserObjectPermission.objects.filter(content_type=ContentType.objects.get_for_model(self),
+                                            object_pk=self.id).delete()
+        GroupObjectPermission.objects.filter(content_type=ContentType.objects.get_for_model(self),
+                                             object_pk=self.id).delete()
+
+    def set_default_permissions(self):
+        """
+        Set default permissions.
+        """
+        # remove all permissions
+        self.remove_object_permissions()
+
+        # default permissions for anonymous users
+        anonymous_group, created = Group.objects.get_or_create(name='anonymous')
+
+        # assign permissions to 'Anyone' if 'DEFAULT_ANONYMOUS_VIEW_PERMISSION' is True
+        if settings.DEFAULT_ANONYMOUS_VIEW_PERMISSION:
+            assign_perm('access_geocollection', anonymous_group, self)
+
+        # default permissions to the Geocollection group members
+        assign_perm('access_geocollection', self.group.group, self)
+
+    def set_permissions(self, perm_spec):
+        anonymous_group = Group.objects.get(name='anonymous')
+        self.remove_object_permissions()
+
+        if 'users' in perm_spec and "AnonymousUser" in perm_spec['users']:
+            if perm_spec['users']['AnonymousUser']:
+                assign_perm(perm_spec['users']['AnonymousUser'], anonymous_group, self)
+
+        if 'users' in perm_spec:
+            for username, perms in perm_spec['users'].items():
+                if get_user_model().objects.filter(username=username).exists():
+                    user = get_user_model().objects.get(username=username)
+                    logger.error(f" assign_perm '{user}' -> {perms}")
+                    for perm in perms:
+                        assign_perm(perm, user, self)
+
+        if 'groups' in perm_spec:
+            for group, perms in perm_spec['groups'].items():
+                group = Group.objects.get(name=group)
+                for perm in perms:
+                    assign_perm(perm, group, self)
+
+    def get_all_level_info(self):
+        resource = self.get_self_resource()
+        users = self.get_users_with_perms()
+        groups = get_groups_with_perms(
+            resource,
+            attach_perms=True)
+        if groups:
+            for group in groups:
+                try:
+                    group_profile = GroupProfile.objects.get(slug=group.name)
+                    managers = group_profile.get_managers()
+                    if managers:
+                        for manager in managers:
+                            if manager not in users and not manager.is_superuser and manager != resource.owner:
+                                assign_perm('access_geocollection', manager, resource)
+                                users[manager] = ['access_geocollection', ]
+                except GroupProfile.DoesNotExist:
+                    tb = traceback.format_exc()
+                    logger.debug(tb)
+        if resource.group:
+            try:
+                group_profile = resource.group
+                managers = group_profile.get_managers()
+                if managers:
+                    for manager in managers:
+                        if manager not in users and not manager.is_superuser and manager != resource.owner:
+                            assign_perm('access_geocollection', manager, resource)
+                            users[manager] = ['access_geocollection', ]
+            except GroupProfile.DoesNotExist:
+                tb = traceback.format_exc()
+                logger.debug(tb)
+
+        info = {
+            'users': users,
+            'groups': groups
+        }
+
+        return info
+
+    def get_self_resource(self):
+        try:
+            if hasattr(self, "resourcebase_ptr_id"):
+                return self.resourcebase_ptr
+        except ObjectDoesNotExist:
+            pass
+        return self
+
     def __str__(self):
         return self.name
 
+    class Meta:
+        permissions = (
+            ('access_geocollection', 'Can view geocollection'),
+        )
```

- Let's test the `set_permissions`

```shell
./manage_dev.sh shell
```
```python
Python 3.8.10 (default, Jun  2 2021, 10:49:15) 
Type 'copyright', 'credits' or 'license' for more information
IPython 7.24.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from geocollections.models import Geocollection

In [2]: Geocollection.objects.first().set_default_permissions()

In [3]: Geocollection.objects.first().get_all_level_info()
Out[3]: 
{'users': {},
 'groups': {<Group: anonymous>: ['access_geocollection'],
  <Group: registered-members>: ['access_geocollection']}}

In [4]: Geocollection.objects.first().remove_object_permissions()

In [5]: Geocollection.objects.first().get_all_level_info()
Out[5]: {'users': {}, 'groups': {}}

In [6]: perm_spec = {
   ...:     "users": {
   ...:         "AnonymousUser": [],
   ...:         "test_user1": ["access_geocollection"],
   ...:         "test_user2": [],
   ...:     },
   ...:     "groups": {
   ...:         "registered-members": ["access_geocollection"]
   ...:     }
   ...: }

In [7]: Geocollection.objects.first().set_permissions(perm_spec)
 assign_perm 'AnonymousUser' -> []
 assign_perm 'test_user1' -> ['access_geocollection']
 assign_perm 'test_user2' -> []

In [8]: Geocollection.objects.first().get_all_level_info()
Out[8]: 
{'users': {<Profile: test_user1>: ['access_geocollection']},
 'groups': {<Group: registered-members>: ['access_geocollection']}}

In [9]: quit()
```

#### Permissions Views and Urls
- Let's use the `access_geocollection` permissions to control access to the views.
- We will also define a specific view allowing us to check/set the geocollection permissions.

```shell
vim geocollections/views.py
```
```diff
--- geocollections/views.py.org	2021-09-13 23:36:59.410056180 +0100
+++ geocollections/views.py	2021-09-13 23:52:22.977934917 +0100
@@ -1,6 +1,50 @@
+import json
+
+from django.http import HttpResponse
 from django.views.generic import DetailView
+from django.core.exceptions import PermissionDenied
+from django.contrib.auth.mixins import PermissionRequiredMixin
 
 from .models import Geocollection
 
-class GeocollectionDetail(DetailView):
+
+class GeocollectionDetail(PermissionRequiredMixin, DetailView):
     model = Geocollection
+
+    def has_permission(self):
+        return self.request.user.has_perm('geocollection.access_geocollection', self)
+
+
+def geocollection_permissions(request, collection_name):
+
+    geocollection = Geocollection.objects.get(name=collection_name)
+    user = request.user
+
+    if user.has_perm('access_geocollection', geocollection):
+        return HttpResponse(
+            (f'You have the permission to access the geocollection "{collection_name}". '
+             'Please customize a template for this view'),
+            content_type='text/plain')
+    else:
+        raise PermissionDenied
+
+    if request.method == 'POST':
+        success = True
+        message = "Permissions successfully updated!"
+        try:
+            perm_spec = json.loads(request.body)
+            geocollection.set_permissions(perm_spec)
+
+            return HttpResponse(
+                json.dumps({'success': success, 'message': message}),
+                status=200,
+                content_type='text/plain'
+            )
+        except Exception as e:
+            success = False
+            message = f"Error updating permissions :(... error: {e}"
+            return HttpResponse(
+                json.dumps({'success': success, 'message': message}),
+                status=500,
+                content_type='text/plain'
+            )
```

- Let's add an `urlpattern` to access the `geocollection_permissions` view.

```shell
vim geocollections/urls.py
```
```diff
--- geocollections/urls.py.org	2021-09-13 23:43:40.534056180 +0100
+++ geocollections/urls.py	2021-09-13 23:46:45.172949596 +0100
@@ -1,9 +1,12 @@
 from django.conf.urls import url
 
-from .views import GeocollectionDetail
+from .views import GeocollectionDetail, geocollection_permissions
 
 urlpatterns = [
     url(r'^(?P<slug>[-\w]+)/$',
         GeocollectionDetail.as_view(),
         name='geocollection-detail'),
+    url(r'^permissions/(?P<collection_name>\w+)$',
+        geocollection_permissions,
+        name='geocollection_permissions')
 ]
```

- Trying to access the views as an `admin` we will be able to get both the details and the permissions check

   ### admin
     ![image](https://user-images.githubusercontent.com/1278021/133168106-79bfd999-7cac-4746-bda4-bbb221f27dcf.png)

     ![image](https://user-images.githubusercontent.com/1278021/133168162-3d4ba427-6b78-405b-b688-59d89636ce1c.png)

   ### anonymous
     ![image](https://user-images.githubusercontent.com/1278021/133168211-b9c3706f-35ce-41f9-a3f6-86f3e01437ad.png)

     ![image](https://user-images.githubusercontent.com/1278021/133168248-70714492-1b87-46c3-b34d-7554ea541a4a.png)

     ![image](https://user-images.githubusercontent.com/1278021/133168295-3f1c1d89-26a2-4ac5-89eb-f2098e9113a1.png)

### [Next Section: Add Tanslations to geonode-project](GEONODE_PROJ_TRX.md)
