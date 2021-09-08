# Introduction to Django and Virtualenvs

_This tutorial is referencing parts and wireframes by_ [A Complete Beginner's Guide to Django - Part 1](https://simpleisbetterthancomplex.com/series/2017/09/04/a-complete-beginners-guide-to-django-part-1.html)

Django is a Web framework written in Python. A Web framework is a software that supports the development of dynamic Web sites, applications, and services. It provides a set of tools and functionalities that solves many common problems associated with Web development, such as security features, database access, sessions, template processing, URL routing, internationalization, localization, and much more.

Using a Web framework, such as Django, enables us to develop secure and reliable Web applications very quickly in a standardized way, without having to reinvent the wheel.

The development of Django is supported by the [Django Software Foundation](https://www.djangoproject.com/foundation/), and it’s sponsored by companies like JetBrains and Instagram. Django has also been around for quite some time now. It’s under active development for more than 12 years now, proving to be a mature, reliable and secure Web framework.

The basic setup consists of installing **Python**, **Virtualenv**, and **Django**.

## Vitualenvs
Using virtual environments is not mandatory, but it’s highly recommended. If you are just getting started, it’s better to start with the right foot.

When developing Web sites or Web projects with Django, it’s very common to have to install external libraries to support the development. Using virtual environments, each project you develop will have its isolated environment. So the dependencies won’t clash. It also allows you to maintain in your local machine projects that run on different Django versions.

- In order to create a `virtualenv`, it is necessary to install some system packages. In a Ubuntu system, as an instance, we can rely on `Python 3.8` and `Virtualenverapper`

```shell
# Install packages from GeoNode core
sudo apt install -y build-essential gdal-bin \
    python3.8-dev python3.8-venv virtualenvwrapper \
    libxml2 libxml2-dev gettext \
    libxslt1-dev libjpeg-dev libpng-dev libpq-dev libgdal-dev \
    software-properties-common build-essential \
    git unzip gcc zlib1g-dev libgeos-dev libproj-dev \
    sqlite3 spatialite-bin libsqlite3-mod-spatialite libsqlite3-dev
```

Since geonode needs a large number of different python libraries and packages, its recommended to use a python virtual environment to avoid conflicts on dependencies with system wide python packages and other installed software. See also documentation of [Virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/stable/) package for more information.

The GeoNode Virtual Environment must be created only the first time. You won’t need to create it again everytime.

- To create a new `virtualenv`

```shell
which python3.8  # copy the path of python executable

# Create the GeoNode Virtual Environment (first time only)
export WORKON_HOME=~/.virtualenvs
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
mkvirtualenv --python=/usr/bin/python3.8 geonode  # Use the python path from above

# Alterantively you can also create the virtual env like below
mkdir -p ~/.virtualenvs
python3.8 -m venv ~/.virtualenvs/geonode
source ~/.virtualenvs/geonode/bin/activate
```

- At this point your command prompt shows a `(geonode)` prefix, this indicates that your virtualenv is active.

- The next time you need to access the Virtual Environment just run

```shell
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
workon geonode

# Alterantively you can also create the virtual env like below
source ~/.virtualenvs/geonode/bin/activate
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
- In order to install Django on a new `virtualenv`, you will need to run the `pip` utility command, e.g.:

```shell
# This will use the same Python version of the virtualenv
pip install Django==<version>
```

- To start a new Django project, run the command below

```shell
django-admin startproject myproject
```

The command-line utility django-admin is automatically installed with Django.

After we run the command above, it will generate the base folder structure for a Django project.

Right now, our myproject directory looks like this:

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

* `manage.py`: a shortcut to use the django-admin command-line utility. It’s used to run management commands related to our project. We will use it to run the development server, run tests, create migrations and much more.
* `__init__.py`: this empty file tells Python that this folder is a Python package.
* `settings.py`: this file contains all the project’s configuration. We will refer to this file all the time!
* `urls.py`: this file is responsible for mapping the routes and paths in our project. For example, if you want to show something in the URL /about/, you have to map it here first.
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

It's important to note that you can't run a Django app without a project. Simple websites like a blog can be written entirely inside a single app, which could be named blog or weblog for example.


#### [Next Section: Preparation of the Dev Environment](DEV_ENV.md)
