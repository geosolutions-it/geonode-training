# Customizing the Look-and-Feel
In this section we will change the look and feel of GeoNode, in particular we will do some customization to help understand how the template inheritance works and how to add new stuff to your GeoNode. The changes will include the home page, the top menu, the footer and a generic GeoNode page.

## Homepage
GeoNode provides some predefined templates to change the home page and the general site content.

In the `/opt/geonode/geonode/templates/` directory we can edit the `index.html`.

Try to edit the content of the `jumbotron box` in the page, save and refresh your browser to see the changes.

```shell
vim geonode/templates/index.html
```
{% raw %}
```diff
diff --git a/geonode/templates/index.html b/geonode/templates/index.html
index 59de01808..bd13da2b2 100644
--- a/geonode/templates/index.html
+++ b/geonode/templates/index.html
@@ -19,7 +19,7 @@
 {% if custom_theme.welcome_theme == 'JUMBOTRON_BG' or not slides %}
 <div class="jumbotron">
   <div class="container gn-container">
-      {% with jumbotron_welcome_title=custom_theme.jumbotron_welcome_title|default:"Welcome"|template_trans %}
+      {% with jumbotron_welcome_title=custom_theme.jumbotron_welcome_title|default:"GeoNode is awsome!!"|template_trans %}
       <h1>{% trans jumbotron_welcome_title %}</h1>
       {% endwith %}
       <p></p>
```
{% endraw %}

![image](https://user-images.githubusercontent.com/1278021/132378649-0dbba09c-9593-4184-9387-8cf04d7d4c90.png)

## Main Theme
To change the main theme of GeoNode we can act on the `base.css` file available in the `/opt/geonode/geonode/static/geonode/css/` folder.

**IMPORTANT**: Remember to put the rules at the end of the file, otherwise they could be overridden by some previous `CSS` rules

**WARNING**: In order to correctly persist the rules, once you are happy with the `CSS`, you will need to transform them into `LESS` and update the `/opt/geonode/geonode/static/geonode/less/base.less` file accordingly. There are plenty of online resources around that will help you doing this.

For example, if we want to change the background of the `jumbotron`, in this file we can add the following rule:

```css
.home .jumbotron { background: red }
```
Adding the `.home` class prefix is necessary in order to let the rule have priority over the GeoNode’s ones.

Please note that, differently than the `.html` files, the `.css` files are static files that need to be preprocessed and moved into a static path.
We do that by invoking the `collectstatic` task:
```bash
./manage_dev.sh collectstatic
```

Once we refresh the page in the browser (you may need to remove the page cache), we should see the change as follows

![image](https://user-images.githubusercontent.com/1278021/132380023-ae8a9a06-5e7f-47eb-85db-f1a61f9c0ab1.png)


## Top Navbar Menu

Let's make some changes that will apply to the whole site. We will add a `Geocollections` entry in the `top menu bar`.

Edit the `geonode_base.html` file in the `/opt/geonode/geonode/base/templates/` folder and add the following lines:

{% raw %}
```django
{% extends "base.html" %}

{% block tabs %}
{{ block.super }}
<li>
 <a href="{{ PROJECT_ROOT }}/geocollections">Geocollections</a>
</li>
{% endblock tabs %}
```
{% endraw %}

Refresh the page and check the outcomes

![image](https://user-images.githubusercontent.com/1278021/132534629-5bc74671-6bd0-47b2-823b-0847363dabc0.png)

In order to practice a bit more with the templates inheritance of Django, try to check what happens if you move the statement `{{ block.super }}` after the list element:

![image](https://user-images.githubusercontent.com/1278021/132535278-22f7bdc4-e8a1-4d9e-83f1-4a938cf01503.png)

and also check what happens if you remove it completely:

![image](https://user-images.githubusercontent.com/1278021/132535379-34222717-ee2c-493e-9462-fe4358a44aa6.png)

This property of Django allows us to completely override the behaviour of parts of the templates accordingly to our needs.

Currently, by clicking over the menu we just created throws an error. This is because the related **view** and **url pattern** do not exist yet. We will create them in the next sections.

# Django templating

You can find the official doc for the Django templating engine at 
https://docs.djangoproject.com/en/2.2/topics/templates/#the-django-template-language

We'll give here some introductory notes about what we saw in the html file we edited.

## [`extends` tag](https://docs.djangoproject.com/en/2.2/ref/templates/builtins/#extends)

Used at the top of an html file, `extends` signals that this template extends a parent template.

`{% raw %}{% extends "base.html" %}{% endraw %}` means that this template [inherits](https://docs.djangoproject.com/en/2.2/ref/templates/language/#template-inheritance) all the content from the file `base.html`.

## [`block` tag](https://docs.djangoproject.com/en/2.2/ref/templates/builtins/#block)

`block` defines a block that can be overridden by child templates.

This is a powerful concept that allows to define a hierarchy of page layouts, where the "lower" level templates just refine some parts in the rendering of a page.

In the file we edited, we `extend`ed  the `base.html` file. Inside `base.html` we have several named `block`s. When we extend a parent template, we can redefine any one of the parent's block, in order to customize the page. This is what we did when we added the `{% raw %}{% block tabs %}{% endraw %}` block in the `geonode_base.html` file: we redefined the content of the `tabs` block defined in the parent template.

## `{{ block.super }}` variable
If you need to get the content of the block from the parent template, the `{{ block.super }}` variable will do the trick. This is useful if you want to add to the contents of a parent block instead of completely overriding it.
That's what we did when redefining the `tabs` block: we reused the previous content of the block of the parent template. By moving the `super` variable before or after the content we added, we could decide where our new content could be placed with respect to the existing content. 


#### [Next Section: Create a geonode project](040_create_project.md)
