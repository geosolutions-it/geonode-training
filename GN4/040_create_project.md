# Create a geonode project

The [`geonode-project` repository](https://github.com/GeoNode/geonode-project/tree/3.3.x) contains a Django project **template** allowing us to create a brand new web project which _has GeoNode core as a dependency_.

In simple words, through this project template we are able to customize somehow our GeoNode instance without actually touching the GeoNode core code.

Thanks to the power of Django, we are able to override **html templates**, **views** and, within certain limits, the **models**.

We'll be calling a **geonode project** (or _`geonode-project` instance_) a Django project created using the `geonode-project` template. 

In this section we will learn how to:

 - Create a new virtual environment for our `geonode-project` instance
 - Initialize a new geonode project `my_geonode`
 - Modify the look-and-feel of the `my_geonode` project


## Create `my_geonode` Django project

First of all we need to create a new virtualenv based on Python 3.8:

```bash 
mkvirtualenv -p $(which python3.8) my_geonode
```
<details>
  <summary>command output</summary>
<pre>
created virtual environment CPython3.8.10.final.0-64 in 385ms
  creator CPython3Posix(dest=/home/geonode-vm-321/.virtualenvs/my_geonode, clear=False, global=False)
  seeder FromAppData(download=False, pip=latest, setuptools=latest, wheel=latest, pkg_resources=latest, via=copy, app_data_dir=/home/geonode-vm-321/.local/share/virtualenv/seed-app-data/v1.0.1.debian.1)
  activators BashActivator,CShellActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator
virtualenvwrapper.user_scripts creating /home/geonode-vm-321/.virtualenvs/my_geonode/bin/predeactivate
virtualenvwrapper.user_scripts creating /home/geonode-vm-321/.virtualenvs/my_geonode/bin/postdeactivate
virtualenvwrapper.user_scripts creating /home/geonode-vm-321/.virtualenvs/my_geonode/bin/preactivate
virtualenvwrapper.user_scripts creating /home/geonode-vm-321/.virtualenvs/my_geonode/bin/postactivate
virtualenvwrapper.user_scripts creating /home/geonode-vm-321/.virtualenvs/my_geonode/bin/get_env_details
(my_geonode) geonode-vm-321@geonodevm-3:~$
</pre>
</details>

Install the correct version of Django
```shell
pip install Django==2.2.20
```
<details>
  <summary>command output</summary>

<pre>
Collecting Django==2.2.20
  Downloading Django-2.2.20-py3-none-any.whl (7.5 MB)
     |████████████████████████████████| 7.5 MB 3.5 MB/s 
Collecting sqlparse>=0.2.2
  Downloading sqlparse-0.4.1-py3-none-any.whl (42 kB)
     |████████████████████████████████| 42 kB 508 kB/s 
Collecting pytz
  Downloading pytz-2021.1-py2.py3-none-any.whl (510 kB)
     |████████████████████████████████| 510 kB 3.3 MB/s 
Installing collected packages: sqlparse, pytz, Django
Successfully installed Django-2.2.20 pytz-2021.1 sqlparse-0.4.1
</pre>
</details>

The new VirtualEnv called `my_geonode` is now ready to be used.

Let's create the target folder with the right permissions for the geonode project:
```shell
cd /opt
sudo mkdir geonode-project
sudo chown -Rf geonode: geonode-project/
cd geonode-project/
```

Now clone the geonode-project template from the repository:
```shell
git clone https://github.com/GeoNode/geonode-project.git -b 3.3.x
```

Create the `my_django` project by using the `geonode-project` as a template:
```shell
django-admin startproject --template=./geonode-project -e py,sh,md,rst,json,yml,ini,env,sample,properties -n monitoring-cron -n Dockerfile my_geonode
```

The previous command line should create a new folder called `my_geonode` containing a Django project instance

<details>
  <summary>Content of "my_geonode/" directory</summary>

<pre>
/opt/geonode-project/my_geonode$ ll
total 220
drwxrwxr-x 7 geonode geonode  4096 Sep  9 15:43 ./
drwxr-xr-x 3 geonode geonode  4096 Sep  9 15:43 ../
-rwxr-xr-x 1 geonode geonode  1261 Sep  9 15:43 celery-cmd*
-rw-rw-r-- 1 geonode geonode   524 Sep  9 15:43 celery.sh
-rw-rw-r-- 1 geonode geonode   644 Sep  9 15:43 dev_config.yml
drwxrwxr-x 6 geonode geonode  4096 Sep  9 15:43 docker/
-rwxr-xr-x 1 geonode geonode    98 Sep  9 15:43 docker-build.sh*
-rw-rw-r-- 1 geonode geonode  6887 Sep  9 15:43 docker-compose.development.yml
-rw-rw-r-- 1 geonode geonode   685 Sep  9 15:43 docker-compose.override.yml
-rw-rw-r-- 1 geonode geonode  4720 Sep  9 15:43 docker-compose.yml
-rw-rw-r-- 1 geonode geonode  3000 Sep  9 15:43 Dockerfile
-rwxr-xr-x 1 geonode geonode   177 Sep  9 15:43 docker-purge.sh*
-rwxr-xr-x 1 geonode geonode  3121 Sep  9 15:43 entrypoint.sh*
-rw-rw-r-- 1 geonode geonode  7138 Sep  9 15:43 .env
drwxrwxr-x 2 geonode geonode  4096 Sep  9 15:43 fixtures/
-rw-rw-r-- 1 geonode geonode   829 Sep  9 15:43 .gitignore
-rw-rw-r-- 1 geonode geonode   506 Sep  9 15:43 jetty-runner.xml
-rw-rw-r-- 1 geonode geonode   559 Sep  9 15:43 Makefile
-rw-rw-r-- 1 geonode geonode    52 Sep  9 15:43 manage_dev.sh.sample
-rwxr-xr-x 1 geonode geonode  1092 Sep  9 15:43 manage.py*
-rwxr-xr-x 1 geonode geonode    77 Sep  9 15:43 manage.sh*
-rw-rw-r-- 1 geonode geonode   256 Sep  9 15:43 monitoring-cron
drwxrwxr-x 5 geonode geonode  4096 Sep  9 15:43 my_geonode/           <------- Django main app folder
-rw-rw-r-- 1 geonode geonode  3308 Sep  9 15:43 .override_dev_env.sample
drwxrwxr-x 3 geonode geonode  4096 Sep  9 15:43 package/
-rw-rw-r-- 1 geonode geonode 41964 Sep  9 15:43 pavement.py
-rw-rw-r-- 1 geonode geonode    40 Sep  9 15:43 paver_dev.sh.sample
-rwxr-xr-x 1 geonode geonode    31 Sep  9 15:43 paver.sh*
-rw-rw-r-- 1 geonode geonode  1566 Sep  9 15:43 playbook.yml
-rw-rw-r-- 1 geonode geonode  6173 Sep  9 15:43 README.md
-rw-rw-r-- 1 geonode geonode    80 Sep  9 15:43 requirements.txt       <------- Django dependencies
drwxrwxr-x 3 geonode geonode  4096 Sep  9 15:43 scripts/
-rw-rw-r-- 1 geonode geonode  1648 Sep  9 15:43 setup.py
-rw-rw-r-- 1 geonode geonode 22993 Sep  9 15:43 tasks.py
-rw-rw-r-- 1 geonode geonode  2445 Sep  9 15:43 uwsgi.ini
-rwxr-xr-x 1 geonode geonode   691 Sep  9 15:43 wait-for-databases.sh*
</pre>
</details>

By taking a look into the `/opt/geonode-project/my_geonode/my_geonode` you should be able to recognize a standard Django app structure

```shell
drwxrwxr-x 5 geonode geonode 4096 Sep  9 15:43 ./
drwxrwxr-x 7 geonode geonode 4096 Sep  9 15:43 ../
-rw-rw-r-- 1 geonode geonode 1279 Sep  9 15:43 apps.py
drwxrwxr-x 2 geonode geonode 4096 Sep  9 15:43 br/
-rw-rw-r-- 1 geonode geonode 1356 Sep  9 15:43 celeryapp.py
-rw-rw-r-- 1 geonode geonode 1042 Sep  9 15:43 __init__.py
-rw-rw-r-- 1 geonode geonode 5000 Sep  9 15:43 settings.py   <------- Django settings
drwxrwxr-x 6 geonode geonode 4096 Sep  9 15:43 static/
drwxrwxr-x 2 geonode geonode 4096 Sep  9 15:43 templates/    <------- HTML templates
-rw-rw-r-- 1 geonode geonode 1234 Sep  9 15:43 urls.py       <------- main URL patterns
-rw-rw-r-- 1 geonode geonode 1779 Sep  9 15:43 version.py
-rw-rw-r-- 1 geonode geonode 1980 Sep  9 15:43 wsgi.py
```

Some files are still missing, like the **views**, **models** and **migrations**, since they are not needed by default.


## Install dependencies

We need to install the geonode project dependencies first.

The default `requirements.txt` file contains two dependencies, that is
- core GeoNode
- `geonode_mapstore_client`: the adapter for using mapstore in GeoNode

Performing a `pip install -r reqs.txt` would install these sources inside the virtual environment hidden directory, but we need our copy of the sources to be installed (remember, we need a setup for developers!)

Edit the file `requirements.txt` and comment out the line
```
#-e git+https://github.com/GeoNode/geonode.git@3.3.x#egg=GeoNode
```

then install the other listed requirements:
```shell
pip install -r requirements.txt
```

Then install the local GeoNode in development mode
```shell
pip install -e /opt/geonode
```

As part of the GeoNode setup, you need to install manually some requirements bound to locally installed binary packages:
```shell
pip install pygdal=="`gdal-config --version`.*"
```

Finally install the current app in dev mode
```shell
pip install -e .
```

Should have we needed to work on the `geonode_mapstore_client`, we would avoid completely to use the `requirements.txt` file, by cloning and installing manually both requirements.  

A **warning** about the editing of the `requirements.txt` file: please note that when releasing your geonode project, the geonode requirement should be in there, so we need to uncomment the line we commented out before pushing our files to a git repository.  
An alternative way to avoid using the geonode version declared in the `requirements.txt` file, without the need to edit it, would have been:
- `pip install` the original `requirements.txt` file
- `pip uninstall GeoNode`
- and then, same as the above task, `pip install -e /opt/geonode`  

Anyway keep in mind that this alternative procedure requires to download geonode sources twice.


```shell
# Ensure the NGINX and UWSGI services have been stopped
sudo systemctl stop nginx
sudo pkill -9 -f uwsgi
sudo ps aux | grep uwsgi

# Ensure we are in the correct virtualenv and folder
workon my_geonode
cd /opt/geonode-project/my_geonode/
```

- Let's edit the project `.env` in order to match the dev environment

## Configuration

The GeoNode settings are configurable using environment variables.
There is a sample file containing the most common variables that may need to be customized.

Copy that sample file into a file we are going to edit: 

```shell
cd /opt/geonode-project/my_geonode
cp .override_dev_env.sample .override_dev_env
```
Also copy the sample files used to run the admin commands:
```shell
cp paver_dev.sh.sample paver_dev.sh
cp manage_dev.sh.sample manage_dev.sh

chmod +x paver_dev.sh manage_dev.sh
```

Edit the file `.override_dev_env` and make sure the database info are correct.
Also, add a line
```bash
export ASYNC_SIGNALS=False
```

This is a recap of the edited lines:
```shell

(my_geonode) geonode@geonode-32x-vm:/opt/geonode-project/my_geonode$ diff .override_dev_env.sample .override_dev_env
```

```diff
10c10
< export GEONODE_DATABASE=my_geonode
---
> export GEONODE_DATABASE=geonode
12c12
< export GEONODE_GEODATABASE=my_geonode_data
---
> export GEONODE_GEODATABASE=geonode_data
15,16c15,16
< export DATABASE_URL=postgis://my_geonode:geonode@localhost:5432/my_geonode
< export GEODATABASE_URL=postgis://my_geonode_data:geonode@localhost:5432/my_geonode_data
---
> export DATABASE_URL=postgis://geonode:geonode@localhost:5432/geonode
> export GEODATABASE_URL=postgis://geonode_data:geonode@localhost:5432/geonode_data
96a97,98
> 
> export ASYNC_SIGNALS=False
```

## Align the DB and GeoNode

```shell
./paver_dev.sh sync
```

**WARNING**: _Whenever you face some issues with the migrations, please try upgrading the dependency as shown below_

```shell
pip install geonode-oauth-toolkit==2.2.2
```

Align the internal URLs and Metadata links

```shell
# The order is important! Those are regex expressions and will be executed one after the other...

# Fix GeoServer URLs first
./manage_dev.sh migrate_baseurl --source-address=http://localhost/geoserver --target-address=http://localhost:8080/geoserver

# Fix GeoNode URLs
./manage_dev.sh migrate_baseurl --source-address=http://localhost/ --target-address=http://localhost:8000/

# Align the Metadata links
./manage_dev.sh set_all_layers_metadata -d
```

## Run GeoNode 

Finally, start `my_geonode` with either
```shell
./manage_dev.sh runserver 0.0.0.0:8000
```
or (run in background)
```shell
./paver_dev.sh start_django
```

- **Notice** that we don't need to fix GeoServer `OAuth2` endpoints since we did already on the previous section
- Open the browser and go to the location `http://localhost:8000`
- The main page and theme have been changed again

![image](https://user-images.githubusercontent.com/1278021/132838719-068a6cd4-65df-43b7-81b0-853293b628b3.png)

#### [Next Section: Customize a geonode project UI](050_project_customize_ui.md)
