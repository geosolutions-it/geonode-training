# Introduction to Django

_This tutorial is referencing parts and wireframes by_ [A Complete Beginner's Guide to Django - Part 1](https://simpleisbetterthancomplex.com/series/2017/09/04/a-complete-beginners-guide-to-django-part-1.html)

Django is a Web framework written in Python. A Web framework is a software that supports the development of dynamic Web sites, applications, and services. It provides a set of tools and functionalities that solves many common problems associated with Web development, such as security features, database access, sessions, template processing, URL routing, internationalization, localization, and much more.

Using a Web framework, such as Django, enables us to develop secure and reliable Web applications very quickly in a standardized way, without having to reinvent the wheel.

The development of Django is supported by the [Django Software Foundation](https://www.djangoproject.com/foundation/), and it’s sponsored by companies like JetBrains and Instagram. Django has also been around for quite some time now. It’s under active development for more than 12 years now, proving to be a mature, reliable and secure Web framework.

The basic setup consists of installing **Python**, **Virtualenv**, and **Django**.

## Virtual environments
Using virtual environments (_virtualenvs_) is not mandatory, but it’s highly recommended. If you are just getting started, it’s better to start with the right foot.

When developing Web sites or Web projects with Django, it’s very common to have to install external libraries to support the development. Using virtual environments, each project you develop will have its isolated environment, so that the dependencies won’t clash. It also allows you to maintain in your local machine projects that run on different Django versions.

- In order to create a `virtualenv`, it is necessary to install some system packages. In a Ubuntu system, as an instance, we can rely on `Python 3.8` and `Virtualenverapper`

```shell
# Install packages from GeoNode core
sudo add-apt-repository ppa:ubuntugis/ppa
sudo apt update -y
sudo apt install -y --allow-downgrades build-essential \
    python3-gdal=3.3.2+dfsg-2~focal2 gdal-bin=3.3.2+dfsg-2~focal2 libgdal-dev=3.3.2+dfsg-2~focal2 \
    python3.8-dev python3.8-venv virtualenvwrapper \
    libxml2 libxml2-dev gettext libmemcached-dev zlib1g-dev \
    libxslt1-dev libjpeg-dev libpng-dev libpq-dev \
    software-properties-common build-essential \
    git unzip gcc zlib1g-dev libgeos-dev libproj-dev \
    sqlite3 spatialite-bin libsqlite3-mod-spatialite libsqlite3-dev
```

Since GeoNode needs a large number of different python libraries and packages, it's recommended to use a python virtual environment to avoid conflicts on dependencies with system-wide python packages and other installed software. See also documentation of [Virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/stable/) package for more information.

The GeoNode Virtual Environment must be created only the first time. You won’t need to create it again everytime.

- To create a new `virtualenv`

```shell
which python3.8  # copy the path of python executable

# Create a test Virtual Environment (first time only)
export WORKON_HOME=~/.virtualenvs
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
mkvirtualenv --python=/usr/bin/python3.8 boards  # Use the python path from above

# Alternatively you can also create the virtual env like below
mkdir -p ~/.virtualenvs
python3.8 -m venv ~/.virtualenvs/boards
source ~/.virtualenvs/boards/bin/activate
```

- At this point your command prompt shows a `(boards)` prefix, this indicates that your virtualenv is active.

- The next time you need to access the Virtual Environment just run

```shell
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
workon boards

# Alternatively you can also activate the virtual env like below
source ~/.virtualenvs/boards/bin/activate
```

- In order to save permanently the virtualenvwrapper environment

```shell
vim ~/.bashrc

# Write to the bottom of the file the following lines
export WORKON_HOME=~/.virtualenvs
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
```

- Whenever you switch project, or you need to create a new GeoNode or `geonode-project` instance, make sure to create and activate a specific `virtualenv` for that one.

## Django
- In order to install Django on a new `virtualenv`, you will need to run the `pip` utility command, in the format `pip install Django==<version>`, e.g.:

```shell
# This will use the same Python version of the virtualenv
pip install Django==2.2.20
```

- To start a new Django project, run the command below

```shell
django-admin startproject myproject
```

The command-line utility `django-admin` is automatically installed with Django.

After we run the command above, it will generate the base folder structure for a Django project.

Right now, our `myproject` directory looks like this:

```shell
myproject/                  <-- higher level folder
 |-- myproject/             <-- django project folder
 |    |-- myproject/
 |    |    |-- __init__.py
 |    |    |-- settings.py
 |    |    |-- urls.py
 |    |    |-- wsgi.py
 |    +-- manage.py
 +-- venv/                  <-- virtual environment folder
```

Our initial project structure is composed of five files:

* `manage.py`: a shortcut to use the `django-admin` command-line utility. It’s used to run management commands related to our project. We will use it to run the development server, run tests, create migrations and much more.
* `__init__.py`: this empty file tells Python that this folder is a Python package.
* `settings.py`: this file contains all the project’s configuration. We will refer to this file all the time!
* `urls.py`: this file is responsible for mapping the routes and paths in our project. For example, if you want to show something in the URL `/about/`, you have to map it here first.
* `wsgi.py`: this file is a simple gateway interface used for deployment. You don’t have to bother about it. Just let it be for now.

Django comes with a simple web server installed. It’s very convenient during the development, so we don’t have to install anything else to run the project locally. We can test it by executing the command:

```shell
python manage.py runserver
```

For now, you can ignore the migration errors; we will get to that later.

Now open the following URL in a Web browser: `http://127.0.0.1:8000` and you should see the following page:

![image](https://user-images.githubusercontent.com/1278021/132476355-b57073a1-72fd-4ab5-b149-b3fb5241b087.png)

Hit `CTRL + C` to stop the development server.

## Django Apps
In the Django philosophy we have two important concepts:

* `app`: is a Web application that does something. An app usually is composed of a set of models (database tables), views, templates, tests.
* `project`: is a collection of configurations and apps. One project can be composed of multiple apps, or a single app.

It's important to note that you can't run a Django **app** without a **project**. Simple websites like a blog can be written entirely inside a single app, which could be named **blog** or **weblog** for example.

![image](https://user-images.githubusercontent.com/1278021/132478639-3c53c2c3-b8e7-499f-a2d4-18c850884dcc.png)

It’s a way to organize the source code. In the beginning, it’s not very trivial to determine what is an app or what is not. How to organize the code and so on. But don’t worry much about that right now! Let’s first get comfortable with Django’s API and the fundamentals.

Alright! So, to illustrate let’s create a simple Web Forum or Discussion Board. To create our first app, go to the directory where the `manage.py` file is and execute the following command:

```shell
django-admin startapp boards
```

Notice that we used the command `startapp` this time.

This will give us the following directory structure:

```shell
myproject/
 |-- myproject/
 |    |-- boards/                <-- our new django app!
 |    |    |-- migrations/
 |    |    |    +-- __init__.py
 |    |    |-- __init__.py
 |    |    |-- admin.py
 |    |    |-- apps.py
 |    |    |-- models.py
 |    |    |-- tests.py
 |    |    +-- views.py
 |    |-- myproject/
 |    |    |-- __init__.py
 |    |    |-- settings.py
 |    |    |-- urls.py
 |    |    |-- wsgi.py
 |    +-- manage.py
 +-- venv/
```

So, let’s first explore what each file does:

* `migrations/`: here Django store some files to keep track of the changes you create in the `models.py` file, so to keep the database and the `models.py` synchronized.
* `admin.py`: this is a configuration file for a built-in Django app called `Django Admin`.
* `apps.py`: this is a configuration file of the app itself.
* `models.py`: here is where we define the entities of our Web application. The models are translated automatically by Django into database tables.
* `tests.py`: this file is used to write unit tests for the app.
* `views.py`: this is the file where we handle the request/response cycle of our Web application.

Now that we created our first app, let’s configure our project to _use_ it.

`settings.py`

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

As you can see, Django already come with 6 built-in apps installed. They offer common functionalities that most Web applications need, like authentication, sessions, static files management (images, javascripts, css, etc.) and so on.

We will explore those apps as we progress in this tutorial series. But for now, let them be and just add our **boards** app to the list of `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'boards',
]
```

Using the analogy of the square and circles from the previous comic, the yellow circle would be our `boards` app, and the `django.contrib.admin`, `django.contrib.auth`, etc, would be the red circles.

## Hello, World!
Let’s write our first **view**. We will explore it in great detail in the next tutorial. But for now, let’s just experiment how it looks like to create a new page with Django.

Open the `views.py` file inside the **boards** app, and add the following code:

`views.py`

```python
from django.http import HttpResponse

def home(request):
    return HttpResponse('Hello, World!')
```

Views are Python functions that receive an `HttpRequest` object and returns an `HttpResponse` object. Receive a `request` as a parameter and returns a `response` as a result. That’s the flow you have to keep in mind!

So, here we defined a simple **view** called `home` which simply returns a message saying `Hello, World!`.

Now we have to tell Django _when to serve this view_. It’s done inside the `urls.py` file:

`urls.py`

```python
from django.contrib import admin
from django.urls import path
from boards import views

urlpatterns = [
    path('', views.home, name='home'),
    path('^admin/', admin.site.urls),
]
```

Comparing this snippet with the original `urls.py` file, you will notice two added lines:
- `path('', views.home, name='home'),`: the empty path `''`is bound to the `home` view
- `from boards import views`: just an import that tells which is the referenced `views.home`.

If we wanted to match the URL `http://127.0.0.1:8000/homepage/`, our url would be: `path('^homepage/$', views.home, name='home')`.

This is just a really simple example. In a similar way we could match URLs defined using regular expressions, or also ask Django to parse parameters embedded in the path (for instance to implement a REST API).

Run the server and let’s see what happen:

```shell
python manage.py runserver
```

In a Web browser, open the `http://127.0.0.1:8000` URL:

![image](https://user-images.githubusercontent.com/1278021/132479724-88d99477-5d4f-414a-ad5f-bffce476e562.png)

That’s it! You just created your very first view.

_Further reading:_ [A Complete Beginner's Guide to Django - Part 2](https://simpleisbetterthancomplex.com/series/2017/09/11/a-complete-beginners-guide-to-django-part-2.html)

#### [Next Section: Deploying a development environment](020_DEV_ENV.md)
