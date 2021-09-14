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
+++ geocollections/models.py	2021-09-14 01:16:41.361828438 +0100
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
@@ -13,6 +27,131 @@
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
+                for perm in perm_spec['users']['AnonymousUser']:
+                   assign_perm(perm, anonymous_group, self)
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

#### Permissions Set View Template
- Let's modify the `geocollection_permissions` view in order to return a `FORM` allowing a user to set the `perm_spec` from the **browser**

```shell
vim geocollections/views.py
```
```diff
--- geocollections/views.py.org	2021-09-13 23:36:59.410056180 +0100
+++ geocollections/views.py	2021-09-14 01:15:30.513828438 +0100
@@ -1,6 +1,56 @@
+import json
+import logging
+import traceback
+
+from django.shortcuts import render
+from django.http import HttpResponse
 from django.views.generic import DetailView
+from django.core.exceptions import PermissionDenied
+from django.contrib.auth.mixins import PermissionRequiredMixin
 
 from .models import Geocollection
 
-class GeocollectionDetail(DetailView):
+logger = logging.getLogger(__name__)
+
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
+    if not user.has_perm('access_geocollection', geocollection):
+        raise PermissionDenied
+
+    if request.method == 'GET':
+        return render(request, 'geocollections/geocollection_permissions.html', context={'object': geocollection})
+
+    elif request.method == 'POST':
+        success = True
+        message = "Permissions successfully updated!"
+        try:
+            perm_spec = json.loads(request.POST.get('perm_spec'))
+            logger.info(f" ---- setting perm_sepc: {perm_spec}")
+            geocollection.set_permissions(perm_spec)
+
+            return HttpResponse(
+                json.dumps({'success': success, 'message': message}),
+                status=200,
+                content_type='text/plain'
+            )
+        except Exception as e:
+            traceback.print_exc()
+            logger.exception(e)
+            success = False
+            message = f"Error updating permissions :(... error: {e}"
+            return HttpResponse(
+                json.dumps({'success': success, 'message': message}),
+                status=500,
+                content_type='text/plain'
+            )
```

- Let's define now the `geocollections/geocollection_permissions.html` template to render and manage the `perm_spec` request

```shell
vim my_geonode/templates/geocollections/geocollection_permissions.html
```
```django
{% extends "geonode_base.html" %}
{% block body %}
    <h2>Geocollection {{ object.name }}</h2>
    <p>You have the permission to access the Geocollection: {{ object.name }}</p>
    <p>Set Permissions:</p>
    <form action="/geocollections/permissions/{{ object.name }}" method="POST" name="geocollections_perm_spec_form">
       {% csrf_token %}
       <label for="perm_spec">Perm Spec: </label><br>
       <textarea id="perm_spec" name="perm_spec" rows=4 cols="50">{"users": {"AnonymousUser": ["access_geocollection"]}, "groups": {}}</textarea><br>
       <input type="submit" value="Submit">
    </form>
{% endblock body%}

{% block extra_script %}
{{ block.super }}
{% endblock extra_script %}
```

- Let's test it; move to `http://localhost:8000/geocollections/permissions/boulder`

    ![image](https://user-images.githubusercontent.com/1278021/133174540-98c77c36-9b94-45d4-a9b3-6d32acfa41c0.png)

- Update the `perm_spec` and click on `Submit`

    ![image](https://user-images.githubusercontent.com/1278021/133174597-1bb994c4-2739-44ec-8d53-94cbb1f13179.png)

- Let's check if the `perm_spec` has changed on the backend

```shell
./manage_dev.sh shell
```
```python
Python 3.8.10 (default, Jun  2 2021, 10:49:15) 
Type 'copyright', 'credits' or 'license' for more information
IPython 7.24.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from geocollections.models import Geocollection

In [2]: Geocollection.objects.get(name='boulder').get_all_level_info()
Out[2]: 
{'users': {<Profile: AnonymousUser>: ['access_geocollection']},
 'groups': {<Group: anonymous>: ['access_geocollection']}}

In [3]: quit()
```

## Adding APIs to Geocollection App
In this section we are going to implement a very simple instance of GeoNode APIs for our Geocollection `app`, along with security integration.

Currently GeoNode provides two types of API endpoints that have been usually identified as `API v1`, provided through [Django Tasypie](https://django-tastypie.readthedocs.io/en/latest/), and `API v2` which are provided by [Django Restframework](https://www.django-rest-framework.org/).

**WARNING** _GeoNode 3.x still provides support for the_ `API v1`. _Those are deprecated and will be dropped in future versions._

### API v1 - Tasypie
- Let's create the `api.py` file first

```shell
vim geocollections/api.py
```
```python
import json
from tastypie.resources import ModelResource
from tastypie import fields
from tastypie.constants import ALL_WITH_RELATIONS, ALL

from geonode.api.api import ProfileResource, GroupResource
from geonode.api.resourcebase_api import ResourceBaseResource

from .models import Geocollection


class GeocollectionResource(ModelResource):

    users = fields.ToManyField(ProfileResource, attribute=lambda bundle: bundle.obj.group.group.user_set.all(), full=True)
    group = fields.ToOneField(GroupResource, 'group__group', full=True)
    resources = fields.ToManyField(ResourceBaseResource, 'resources', full=True)

    class Meta:
        queryset = Geocollection.objects.all().order_by('-group')
        ordering = ['group']
        allowed_methods = ['get']
        resource_name = 'geocollections'
        filtering = {
            'group': ALL_WITH_RELATIONS,
            'id': ALL
        }
```

- API authorization

```shell
vim geocollections/api.py
```
```diff
--- geocollections/api.py.org	2021-09-14 11:02:11.106936710 +0100
+++ geocollections/api.py	2021-09-14 11:02:14.948856713 +0100
@@ -2,6 +2,9 @@
 from tastypie.resources import ModelResource
 from tastypie import fields
 from tastypie.constants import ALL_WITH_RELATIONS, ALL
+from tastypie.authorization import DjangoAuthorization
+
+from guardian.shortcuts import get_objects_for_user
 
 from geonode.api.api import ProfileResource, GroupResource
 from geonode.api.resourcebase_api import ResourceBaseResource
@@ -9,6 +12,21 @@
 from .models import Geocollection
 
 
+class GeocollectionAuth(DjangoAuthorization):
+
+    def read_list(self, object_list, bundle):
+        permitted_ids = get_objects_for_user(
+            bundle.request.user,
+            'geocollections.access_geocollection').values('id')
+
+        return object_list.filter(id__in=permitted_ids)
+
+    def read_detail(self, object_list, bundle):
+        return bundle.request.user.has_perm(
+            'access_geocollection',
+            bundle.obj)
+
+
 class GeocollectionResource(ModelResource):
 
     users = fields.ToManyField(ProfileResource, attribute=lambda bundle: bundle.obj.group.group.user_set.all(), full=True)
@@ -16,6 +34,7 @@
     resources = fields.ToManyField(ResourceBaseResource, 'resources', full=True)
 
     class Meta:
+        authorization = GeocollectionAuth()
         queryset = Geocollection.objects.all().order_by('-group')
         ordering = ['group']
         allowed_methods = ['get']
```

- API urls

```shell
vim my_geonode/urls.py
```
```diff
--- my_geonode/urls.py.org	2021-09-14 11:05:03.377028744 +0100
+++ my_geonode/urls.py	2021-09-14 11:05:54.934794761 +0100
@@ -24,8 +24,15 @@
 from geonode.urls import urlpatterns
 from geonode.base import register_url_event
 
+from geonode.api.urls import api
+
+from geocollections.api import GeocollectionResource
+
+api.register(GeocollectionResource())
+
 urlpatterns += [
 ## include your urls here
+    url(r'', include(api.urls)),
     url(r'^geocollections/', include('geocollections.urls')),
 ]
```

- Let's test them; move to `http://localhost:8000/api/geocollections/`

```json
{"meta": {"limit": 1000, "next": null, "offset": 0, "previous": null, "total_count": 1}, "objects": [{"group": {"group_profile": {"access": "private", "categories": [], "created": "2021-06-30T18:38:07.129648", "description": "", "description_en": null, "detail_url": "/groups/group/registered-members/", "email": null, "id": 1, "last_modified": "2021-06-30T18:38:07.135664", "logo": null, "logo_url": "/static/geonode/img/missing_thumb.png", "manager_count": 0, "member_count": 3, "resource_uri": "/api/group_profile/1/", "slug": "registered-members", "title": "Registered Members", "title_en": "Registered Members"}, "id": 3, "name": "registered-members", "resource_counts": {"all": {"approved": 0, "published": 0, "total": 0, "visible": 0}, "document": {"approved": 0, "published": 0, "total": 0, "visible": 0}, "geoapp": {"approved": 0, "published": 0, "total": 0, "visible": 0}, "geostory": {"approved": 0, "published": 0, "total": 0, "visible": 0}, "layer": {"approved": 0, "published": 0, "total": 0, "visible": 0}, "map": {"approved": 0, "published": 0, "total": 0, "visible": 0}}, "resource_uri": "/api/groups/3/"}, "id": 1, "name": "boulder", "resource_uri": "/api/geocollections/1/", "resources": [{"abstract": "Boulder Colorado", "alternate": null, "attribution": null, "bbox_polygon": null, "category": null, "constraints_other": null, "created": "2021-07-14T15:45:42.514906", "csw_insert_date": "2021-07-15T09:16:50.224808", "csw_mdsource": "local", "csw_schema": "http://www.isotc211.org/2005/gmd", "csw_type": "dataset", "csw_typename": "gmd:MD_Metadata", "csw_wkt_geometry": "POLYGON((-180 -90,-180 90,180 90,180 -90,-180 -90))", "custom_md": null, "data_quality_statement": null, "date": "2021-07-14T15:45:42.494086", "date_type": "publication", "detail_url": "/apps/preview/36", "dirty_state": false, "doi": null, "edition": null, "featured": false, "group": null, "id": 36, "is_approved": true, "is_published": true, "keywords": [], "language": "eng", "last_updated": "2021-07-15T09:16:50.191068", "ll_bbox_polygon": null, "maintenance_frequency": null, "metadata_only": false, "metadata_uploaded": false, "metadata_uploaded_preserve": false, "owner": {"id": 1001, "username": "test_user1"}, "popular_count": 0, "purpose": null, "rating": 0, "regions": ["/api/regions/1/"], "resource_type": "geostory", "resource_uri": "/api/base/36/", "share_count": 0, "srid": "EPSG:4326", "supplemental_information": "No information provided", "temporal_extent_end": null, "temporal_extent_start": null, "thumbnail_url": "http://localhost:8000/static/geonode/img/missing_thumb.png", "title": "Boulder Colorado", "tkeywords": [], "uuid": ""}, {"abstract": "No abstract provided", "alternate": "geonode:BoulderCityLimits", "attribution": null, "bbox_polygon": "SRID=4326;POLYGON ((3055613.75 1229960.625, 3055613.75 1277375.875, 3090085.5 1277375.875, 3090085.5 1229960.625, 3055613.75 1229960.625))", "category": null, "constraints_other": null, "created": "2021-07-13T16:43:58.533229", "csw_insert_date": "2021-07-13T16:44:23.454513", "csw_mdsource": "local", "csw_schema": "http://www.isotc211.org/2005/gmd", "csw_type": "dataset", "csw_typename": "gmd:MD_Metadata", "csw_wkt_geometry": "SRID=4326;POLYGON ((-105.301591603937 39.96417673980012, -105.301591603937 40.09461709343572, -105.1779963136617 40.09461709343572, -105.1779963136617 39.96417673980012, -105.301591603937 39.96417673980012))", "custom_md": null, "data_quality_statement": null, "date": "2021-07-13T16:43:58.509987", "date_type": "publication", "detail_url": "/layers/geonode_data:geonode:BoulderCityLimits", "dirty_state": false, "doi": null, "edition": null, "featured": false, "group": null, "id": 18, "is_approved": true, "is_published": true, "keywords": [], "language": "eng", "last_updated": "2021-07-13T16:44:23.431238", "ll_bbox_polygon": "SRID=4326;POLYGON ((-105.301591603937 39.96445409752658, -105.3012123901718 40.09461709343572, -105.1779963136617 40.09433920944086, -105.1786105751238 39.96417673980012, -105.301591603937 39.96445409752658))", "maintenance_frequency": null, "metadata_only": false, "metadata_uploaded": false, "metadata_uploaded_preserve": false, "owner": {"id": 1001, "username": "test_user1"}, "popular_count": 0, "purpose": null, "rating": 0, "regions": ["/api/regions/1/"], "resource_type": "layer", "resource_uri": "/api/base/18/", "share_count": 0, "srid": "EPSG:2876", "supplemental_information": "No information provided", "temporal_extent_end": null, "temporal_extent_start": null, "thumbnail_url": "http://localhost:8000/uploaded/thumbs/layer-852259fc-e3f9-11eb-a768-47eca398f8be-thumb.png", "title": "BoulderCityLimits", "tkeywords": [], "uuid": "852259fc-e3f9-11eb-a768-47eca398f8be"}, {"abstract": "No abstract provided", "alternate": null, "attribution": null, "bbox_polygon": "SRID=4326;POLYGON ((-180 -90, -180 90, 180 90, 180 -90, -180 -90))", "category": null, "constraints_other": null, "created": "2021-07-14T09:50:11.673661", "csw_insert_date": "2021-07-14T09:50:13.468480", "csw_mdsource": "local", "csw_schema": "http://www.isotc211.org/2005/gmd", "csw_type": "document", "csw_typename": "gmd:MD_Metadata", "csw_wkt_geometry": "SRID=4326;POLYGON ((-180 -90, -180 90, 180 90, 180 -90, -180 -90))", "custom_md": null, "data_quality_statement": null, "date": "2021-07-14T09:50:11.520208", "date_type": "publication", "detail_url": "/documents/32", "dirty_state": false, "doi": null, "edition": null, "featured": false, "group": null, "id": 32, "is_approved": true, "is_published": true, "keywords": [], "language": "eng", "last_updated": "2021-07-14T09:50:13.439131", "ll_bbox_polygon": "SRID=4326;POLYGON ((-180 -90, -180 90, 180 90, 180 -90, -180 -90))", "maintenance_frequency": null, "metadata_only": false, "metadata_uploaded": false, "metadata_uploaded_preserve": false, "owner": {"id": 1001, "username": "test_user1"}, "popular_count": 0, "purpose": null, "rating": 0, "regions": ["/api/regions/1/"], "resource_type": "document", "resource_uri": "/api/base/32/", "share_count": 0, "srid": "4326", "supplemental_information": "No information provided", "temporal_extent_end": null, "temporal_extent_start": null, "thumbnail_url": "/static/geonode/img/missing_thumb.png", "title": "Boulder Colorado", "tkeywords": [], "uuid": "e19b9cae-e488-11eb-8431-fbbd1867106e"}, {"abstract": "No abstract provided", "alternate": "geonode:Buildings050714", "attribution": null, "bbox_polygon": "SRID=4326;POLYGON ((3045967.25 1206627.75, 3045967.25 1285209.75, 3108482.75 1285209.75, 3108482.75 1206627.75, 3045967.25 1206627.75))", "category": null, "constraints_other": null, "created": "2021-07-13T16:51:54.210684", "csw_insert_date": "2021-07-13T16:52:19.256312", "csw_mdsource": "local", "csw_schema": "http://www.isotc211.org/2005/gmd", "csw_type": "dataset", "csw_typename": "gmd:MD_Metadata", "csw_wkt_geometry": "SRID=4326;POLYGON ((-105.3361603091156 39.89992212782436, -105.3361603091156 40.11617646966664, -105.1121150420894 40.11617646966664, -105.1121150420894 39.89992212782436, -105.3361603091156 39.89992212782436))", "custom_md": null, "data_quality_statement": null, "date": "2021-07-13T16:51:54.056350", "date_type": "publication", "detail_url": "/layers/geonode_data:geonode:Buildings050714", "dirty_state": false, "doi": null, "edition": null, "featured": false, "group": null, "id": 19, "is_approved": true, "is_published": true, "keywords": [], "language": "eng", "last_updated": "2021-07-13T16:52:19.232245", "ll_bbox_polygon": "SRID=4326;POLYGON ((-105.3361603091156 39.90045483606045, -105.3356411675245 40.11617646966664, -105.1121150420894 40.11564208499033, -105.1133401975235 39.89992212782436, -105.3361603091156 39.90045483606045))", "maintenance_frequency": null, "metadata_only": false, "metadata_uploaded": false, "metadata_uploaded_preserve": false, "owner": {"id": 1001, "username": "test_user1"}, "popular_count": 0, "purpose": null, "rating": 0, "regions": ["/api/regions/1/"], "resource_type": "layer", "resource_uri": "/api/base/19/", "share_count": 0, "srid": "EPSG:2876", "supplemental_information": "No information provided", "temporal_extent_end": null, "temporal_extent_start": null, "thumbnail_url": "http://localhost:8000/uploaded/thumbs/layer-a07ba8e2-e3fa-11eb-a768-47eca398f8be-thumb.png?v=b512e364", "title": "Buildings050714", "tkeywords": [], "uuid": "a07ba8e2-e3fa-11eb-a768-47eca398f8be"}], "slug": "boulder", "users": [{"avatar_100": "/uploaded/avatars/test_user1/resized/240/flowers.jpg", "documents_count": 5, "first_name": "User1", "id": 1001, "last_name": "Test", "layers_count": 10, "maps_count": 1, "profile_detail_url": "/people/profile/test_user1/", "username": "test_user1"}, {"avatar_100": "https://www.gravatar.com/avatar/6b38efd9db8f3970a3d800b97cb731f3/?s=240", "documents_count": 0, "first_name": "", "id": 1002, "last_name": "", "layers_count": 0, "maps_count": 0, "profile_detail_url": "/people/profile/test_user2/", "username": "test_user2"}]}]}
```

### [Next Section: Add Tanslations to geonode-project](GEONODE_PROJ_TRX.md)
