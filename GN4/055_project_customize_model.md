# Customize a geonode project model

In this section, we will patch the `ResourceBase` of `GeoNode` and update the `templates` in order to add one more field to the `Metadata Schema`.

We will add a `custom_md` field to the `ResourceBase` model and modify the templates in order to show the new field both into the `Metadata Wizard` and the `Layer Details` page.


## Activate the virtualenv

We should already be working in the right directory, and with the proper venv activated.
If not, run:

```shell
workon my_geonode
cd /opt/geonode-project/my_geonode/
```


- Update the GeoNode `ResourceBase` model

```shell
vim /opt/geonode/geonode/base/models.py
```
{% raw %}
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
{% endraw %}

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
{% raw %}
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
{% endraw %}

- Let's check the changes

```shell
./paver_dev.sh start_django
```

![image](https://user-images.githubusercontent.com/1278021/133050035-f86bf268-37fa-44e3-a370-7f4a756ebe6a.png)

- Update the `custom_md` text field to a `rich HTML` one

```shell
vim /opt/geonode/geonode/base/forms.py
```
{% raw %}
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
{% endraw %}

![image](https://user-images.githubusercontent.com/1278021/133050807-bb921fcc-0b1e-413b-8664-3b896b46ceb2.png)

### Update the Layers Details Templates
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
{% raw %}
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
{% endraw %}

![image](https://user-images.githubusercontent.com/1278021/133053625-ad93d6cc-bf87-4430-8ae5-b87dfedc068c.png)

## API REST (v2)
- Let's add the new `custom_md` field to the `REST API` of GeoNode

   The APIs are reachable through the endpoint: `http://localhost:8000/api/v2/`
   
   Looking at the `http://localhost:8000/api/v2/resources` you will notice that the new field we just added is not visible
   
   ![image](https://user-images.githubusercontent.com/1278021/133063348-69beee93-9a10-4c39-b869-69c593eee441.png)

- Modify the GeoNode `base` rest api endpoints in order to add the new field

```shell
vim /opt/geonode/geonode/base/api/serializers.py
```
{% raw %}
```diff
diff --git a/geonode/base/api/serializers.py b/geonode/base/api/serializers.py
index c1a5e8679..909aca07e 100644
--- a/geonode/base/api/serializers.py
+++ b/geonode/base/api/serializers.py
@@ -293,6 +293,7 @@ class ResourceBaseSerializer(BaseDynamicModelSerializer):
         self.fields['raw_data_quality_statement'] = serializers.CharField(read_only=True)
         self.fields['metadata_only'] = serializers.BooleanField()
         self.fields['processed'] = serializers.BooleanField(read_only=True)
+        self.fields['custom_md'] = serializers.CharField()
 
         self.fields['embed_url'] = EmbedUrlField()
         self.fields['thumbnail_url'] = ThumbnailUrlField()
@@ -325,7 +326,8 @@ class ResourceBaseSerializer(BaseDynamicModelSerializer):
             'popular_count', 'share_count', 'rating', 'featured', 'is_published', 'is_approved',
             'detail_url', 'embed_url', 'created', 'last_updated',
             'raw_abstract', 'raw_purpose', 'raw_constraints_other',
-            'raw_supplemental_information', 'raw_data_quality_statement', 'metadata_only', 'processed'
+            'raw_supplemental_information', 'raw_data_quality_statement', 'metadata_only', 'processed',
+            'custom_md'
             # TODO
             # csw_typename, csw_schema, csw_mdsource, csw_insert_date, csw_type, csw_anytext, csw_wkt_geometry,
             # metadata_uploaded, metadata_uploaded_preserve, metadata_xml,
```
{% endraw %}

- `http://localhost:8000/api/v2/resources?filter{custom_md.isnull}=False`

  ![image](https://user-images.githubusercontent.com/1278021/133064948-84e0a153-7cb7-4911-b0a9-42ac286f7c93.png)

- More info about `dynamic-rest` filtering options can be found at: `https://github.com/AltSchool/dynamic-rest#filtering`

#### [Next Section: Save changes to GitHub](060_GEONODE_PROJ_SAVE_GITHUB.md)
