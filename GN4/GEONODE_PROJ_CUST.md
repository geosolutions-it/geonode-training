# Programmatically Customize the geonode-project instance

### Homepage
- Modify some text from the `site_index.html` template


```shell
vim my_geonode/templates/site_index.html
```

```diff
--- my_geonode/templates/site_index.html.org	2021-09-10 11:25:45.185777196 +0100
+++ my_geonode/templates/site_index.html	2021-09-10 11:28:09.255720387 +0100
@@ -9,11 +9,11 @@
 {% if custom_theme.welcome_theme == 'JUMBOTRON_BG' or not slides %}
 <div class="jumbotron">
   <div class="container">
-    {% with jumbotron_welcome_title=custom_theme.jumbotron_welcome_title|default:"Welcome"|template_trans %}
+    {% with jumbotron_welcome_title=custom_theme.jumbotron_welcome_title|default:"My GeoNode is Awesome!!"|template_trans %}
     <h1>{% trans jumbotron_welcome_title %}</h1>
     {% endwith %}
     <p></p>
-    {% with jumbotron_welcome_content=custom_theme.jumbotron_welcome_content|default:"GeoNode is an open source platform for sharing geospatial data and maps."|template_trans %}
+    {% with jumbotron_welcome_content=custom_theme.jumbotron_welcome_content|default:"My GeoNode is an open source platform for sharing geospatial data and maps."|template_trans %}
     <p>{% trans jumbotron_welcome_content %}</p>
     {% endwith %}
     {% if not custom_theme.jumbotron_cta_hide %}
@@ -73,4 +73,4 @@
   </div>
 </div>
 {% endif %}
-{% endblock hero %}
\ No newline at end of file
+{% endblock hero %}
```

![image](https://user-images.githubusercontent.com/1278021/132840646-15e34fda-deef-4112-832b-8ef1b6775db7.png)

### The Theme

To change the theme of our `geonode-project` we can act on the `site_base.css` file available in the `my_geonode/my_geonode/static/css` folder.

The file is empty so we can inspect elements of the home page with the browser's developer tools and define `CSS` rules in there.

For example, if we want to change the `background` of the `jumbotron`, in this file we can add

```css
.home .jumbotron { background: red }
```

Run the `collectstatic` management command in order to allow Django placing the static files on the correct folders.

```shell
./manage_dev.sh collectstatic
```

Then once we refresh the browser, we should see the change as follows:

![image](https://user-images.githubusercontent.com/1278021/132841487-3a42c1f9-9301-4af7-9e3a-c681d25c1e91.png)

Adding the `.home` _CSS class_ is necessary in order to let the rule get precedence over the GeoNode’s ones. 

We can check the rules by inspecting the element in the developer console.

![image](https://user-images.githubusercontent.com/1278021/132841918-184753bb-475e-48fa-b3e2-f59effb543fa.png)

![image](https://user-images.githubusercontent.com/1278021/132842016-d004d93b-a5f0-4fa3-a664-cc24e64edc59.png)


### The Top Menu

Let's make some changes that will apply to the whole site. We can add a `Geocollections` entry in the **top menu bar**.

Edit the `site_base.html` file in the templates folder and **uncomment** the list item adapting the text as well from:

```diff
--- my_geonode/templates/site_base.html.org	2021-09-10 11:46:44.763720387 +0100
+++ my_geonode/templates/site_base.html	2021-09-10 11:47:12.495720387 +0100
@@ -3,10 +3,7 @@
       <link href="{{ STATIC_URL }}css/site_base.css" rel="stylesheet"/>
 {% endblock %}
 {% block extra_tab %}
-{% comment %}
-Add Tab for Third Party Apps
 <li>
- <a href="{{ PROJECT_ROOT }}app">App</a>
+ <a href="{{ PROJECT_ROOT }}geocollections">Geocollections</a>
 </li>
-{% endcomment %}
 {% endblock %}
```

![image](https://user-images.githubusercontent.com/1278021/132842423-465d8e19-69d6-4ab4-9bb5-b0d69f946cdd.png)

### A GeoNode Generic Page

As you can see in the `templates` folder there are only the `site_index.html` and the `site_base.html` files. In order to customize another GeoNode page, for example the **layers list page**, you need to recreate _the same folder structure_ of the GeoNode templates folder and add a file with the same name.

For the layers list page we can create a directory named `layers` inside the `templates` directory and a file named `layer_list.html` inside `layers`.

The changes made in this file will only affect the layer list page.

```shell
mkdir -p my_geonode/templates/layers/
cp /opt/geonode/geonode/layers/templates/layers/layer_list_default.html my_geonode/templates/layers/layer_list_default.html
vim my_geonode/templates/layers/layer_list_default.html

diff -ruN /opt/geonode/geonode/layers/templates/layers/layer_list_default.html my_geonode/templates/layers/layer_list_default.html
```

```diff
--- /opt/geonode/geonode/layers/templates/layers/layer_list_default.html	2021-09-01 14:22:59.778823091 +0100
+++ my_geonode/templates/layers/layer_list_default.html	2021-09-10 11:55:32.195720387 +0100
@@ -2,7 +2,7 @@
 {% load i18n %}
 {% load staticfiles %}
 
-{% block title %} {% trans "Explore Layers" %} - {{ block.super }} {% endblock %}
+{% block title %} {% trans "Explore My Layers" %} - {{ block.super }} {% endblock %}
 
 {% block body_class %}layers explore{% endblock %}
 
@@ -11,7 +11,7 @@
   {% if user.is_authenticated and not READ_ONLY_MODE %}
     <a href="{% url "layer_upload" %}" class="btn btn-primary pull-right">{% trans "Upload Layers" %}</a>
   {% endif %}
-  <h2 class="page-title">{% trans "Explore Layers" %}</h2>
+  <h2 class="page-title">{% trans "Explore My Layers" %}</h2>
 </div>
   {% with include_type_filter='true' %}
     {% with header='Type' %}
```

![image](https://user-images.githubusercontent.com/1278021/132843431-0d00d0e9-770f-4850-8c82-653f3f697c49.png)

### Modify Functionality: Add new Custom Metadata Field

#### Update the Model Base
In this section, we will patch the `ResourceBase` of `GeoNode` and update the `templates` in order to add one more field to the `Metadata Schema`.

We will add a `Custpm Md` field to the `ResourceBase` model and modify the `templates` in order to show the new field both into the `Metadata Wizard` and the `Layer Details` page.

- Prepare the VirtualEnv

```shell
workon my_geonode
cd /opt/geonode-project/my_geonode/
```

- Update the GeoNode `ResourceBase` model

```shell
vim /opt/geonode/geonode/base/models.py
```

```diff
diff --git a/geonode/base/models.py b/geonode/base/models.py
index e2a5a409c..922c67db4 100644
--- a/geonode/base/models.py
+++ b/geonode/base/models.py
@@ -706,6 +706,7 @@ class ResourceBase(PolymorphicModel, PermissionLevelMixin, ItemBase):
     data_quality_statement_help_text = _(
         'general explanation of the data producer\'s knowledge about the lineage of a'
         ' dataset')
+    custom_md_help_text = _('a custom metadata field')
     # internal fields
     uuid = models.CharField(max_length=36)
     title = models.CharField(_('title'), max_length=255, help_text=_(
@@ -959,6 +960,13 @@ class ResourceBase(PolymorphicModel, PermissionLevelMixin, ItemBase):
     __is_approved = False
     __is_published = False
 
+
+    custom_md = models.TextField(
+        _("Custom Md"),
+        blank=True,
+        null=True,
+        help_text=custom_md_help_text)
+
     objects = ResourceBaseManager()
 
     class Meta:
```

- Add the new field to the DB

```shell
./manage_dev.sh makemigrations
./manage_dev.sh migrate
```

- Add the new field to `templates`

```shell
mkdir my_geonode/templates/layouts/
cp /opt/geonode/geonode/layers/templates/layouts/panels.html my_geonode/templates/layouts/panels.html
```

```shell
vim my_geonode/templates/layouts/panels.html
```

```diff
--- /opt/geonode/geonode/layers/templates/layouts/panels.html	2021-09-01 14:22:59.778823091 +0100
+++ my_geonode/templates/layouts/panels.html	2021-09-10 15:48:39.691395977 +0100
@@ -317,6 +317,10 @@
                                 </div>
                                 {% endblock thumbnail %}
                                 <div class="col-lg-4">
+                                    <div id="req_item">
+                                      <span><label for="{{ layer_form.custom_md|id }}">{{ layer_form.custom_md.label }}</label></span>
+                                      {{ layer_form.custom_md }}
+                                    </div>
                                     {% block layer_title %}
                                     <div id="req_item">
                                       <span><label for="{{ layer_form.title|id }}">{{ layer_form.title.label }}</label></span>
```

- Let's check the changes

```shell
./paver_dev.sh start_django
```

![image](https://user-images.githubusercontent.com/1278021/133050035-f86bf268-37fa-44e3-a370-7f4a756ebe6a.png)

- Update the `custom_md` text field to a `rich HTML` one

```shell
vim /opt/geonode/geonode/base/forms.py
```

```diff
diff --git a/geonode/base/forms.py b/geonode/base/forms.py
index 48a0ef7f1..e0284988c 100644
--- a/geonode/base/forms.py
+++ b/geonode/base/forms.py
@@ -416,6 +416,10 @@ class ResourceBaseForm(TranslationModelForm):
         label=_("Data quality statement"),
         required=False,
         widget=TinyMCE())
+    custom_md = forms.CharField(
+        label=_("Custom Md"),
+        required=True,
+        widget=TinyMCE())
 
     owner = forms.ModelChoiceField(
         empty_label=_("Owner"),
```

![image](https://user-images.githubusercontent.com/1278021/133050807-bb921fcc-0b1e-413b-8664-3b896b46ceb2.png)

#### Update the Layers Details Templates
- Copy the default GeoNode `layer_details` template

```shell
mkdir my_geonode/templates/layers
cp /opt/geonode/geonode/layers/templates/layers/layer_detail.html my_geonode/templates/layers/layer_detail.html
```

- Add the new field to the `layer_details` template

```shell
vim my_geonode/templates/layers/layer_detail.html
```

![image](https://user-images.githubusercontent.com/1278021/133052525-a2a46eed-3a81-4d2b-9c88-22e3c639c572.png)

```shell
mkdir my_geonode/templates/base
cp /opt/geonode/geonode/base/templates/base/resourcebase_info_panel.html my_geonode/templates/base/resourcebase_info_panel.html
```

```shell
vim my_geonode/templates/base/resourcebase_info_panel.html
```

![image](https://user-images.githubusercontent.com/1278021/133052924-f7b3c258-972a-44a7-9011-a62d9886bdd1.png)

```shell
cp /opt/geonode/geonode/base/templates/base/_resourcebase_info_panel.html my_geonode/templates/base/_resourcebase_info_panel.html
```

```shell
vim my_geonode/templates/base/_resourcebase_info_panel.html
```

```diff
--- /opt/geonode/geonode/base/templates/base/_resourcebase_info_panel.html	2021-07-14 14:38:25.391987680 +0100
+++ my_geonode/templates/base/_resourcebase_info_panel.html	2021-09-13 09:47:42.768337828 +0100
@@ -31,6 +31,11 @@
     <dd itemprop="description">{{ resource.abstract|safe }}</dd>
     {% endif %}
 
+    {% if resource.custom_md %}
+    <dt>{% trans "Custom Metadata" %}</dt>
+    <dd itemprop="description">{{ resource.custom_md|safe }}</dd>
+    {% endif %}
+
     {% if resource.date %}
     {% if resource.date_type == 'creation' %}
     <dt>{% trans "Creation Date" %}</dt>
```

![image](https://user-images.githubusercontent.com/1278021/133053625-ad93d6cc-bf87-4430-8ae5-b87dfedc068c.png)


#### [Next Section: Save Changes to GitHub](GEONODE_PROJ_SAVE_GITHUB.md)
