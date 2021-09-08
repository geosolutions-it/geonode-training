# Introduction to Django and Virtualenvs
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



#### [Next Section: Preparation of the Dev Environment](DEV_ENV.md)
