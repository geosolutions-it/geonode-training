# Link GeoNode to a geonode-project instance
A `geonode-project` is basically a Django project **template** allowing us to create a brand new web project which _has GeoNode core as a dependency_.

In simple words, through this template project we are able to customize somehow our GeoNode instance without actually touching the GeoNode core code.

Thanks to the prower of Django, we are able to override **templates**, **views** and, withing certain limits, the **models**.

In this section we will learn how to:

 - Create a new `virtualenv` for our `geonode-project` instance
 - Initialize a new `geonode-project` instance called `my_geonode`
 - Modify the look-and-feel from the `my_geonode` instance

## Create `my_geonode` Django Project
- First of all we need to create a new VirtualEnv

```shell
# Let's recognize where the Python 3.8 is located
which python3.8
/usr/bin/python3.8
```
```shell
# Let's create the virtualenv based on Python 3.8
mkvirtualenv -p /usr/bin/python3.8 my_geonode
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
```
```shell
# Let's install the correct version of Django
pip install Django==2.2.20
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
```

- The new VirtialEnv called `my_geonode` is now ready to be used.
- Let's create the `my_django` project by using the `geonode-project` as a template

```shell
# Let's create the target folder first, with the right permissions
cd /opt
sudo mkdir geonode-project
sudo chown -Rf geonode-vm-321: geonode-project/
cd geonode-project/
```
- Let's create the instance by cloning the geonode-project template
- First of all we need to get the correct link to the `geonode-project` template
- Go to `https://github.com/GeoNode/geonode-project` and switch to the `3.2.x` branch

![image](https://user-images.githubusercontent.com/1278021/132707758-69073cd2-cadc-49e9-934e-407166d46f56.png)

- Copy the `zip file` link address

![image](https://user-images.githubusercontent.com/1278021/132708095-6fc12adc-f2c7-4697-b30c-9a2107f1dd60.png)

- Let's finally create the project


```shell
# Let's create the instance by cloning the geonode-project template
django-admin startproject --template=https://github.com/GeoNode/geonode-project/archive/refs/heads/3.2.x.zip -e py,sh,md,rst,json,yml,ini,env,sample,properties -n monitoring-cron -n Dockerfile my_geonode
```

The previous command line should create a new folder called `my_geonode` containing a Django project instance

```shell
/opt/geonode-project/my_geonode$ ll
total 220
drwxrwxr-x 7 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 ./
drwxr-xr-x 3 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 ../
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321  1261 Sep  9 15:43 celery-cmd*
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   524 Sep  9 15:43 celery.sh
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   644 Sep  9 15:43 dev_config.yml
drwxrwxr-x 6 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 docker/
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321    98 Sep  9 15:43 docker-build.sh*
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  6887 Sep  9 15:43 docker-compose.development.yml
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   685 Sep  9 15:43 docker-compose.override.yml
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  4720 Sep  9 15:43 docker-compose.yml
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  3000 Sep  9 15:43 Dockerfile
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321   177 Sep  9 15:43 docker-purge.sh*
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321  3121 Sep  9 15:43 entrypoint.sh*
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  7138 Sep  9 15:43 .env
drwxrwxr-x 2 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 fixtures/
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   829 Sep  9 15:43 .gitignore
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   506 Sep  9 15:43 jetty-runner.xml
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   559 Sep  9 15:43 Makefile
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321    52 Sep  9 15:43 manage_dev.sh.sample
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321  1092 Sep  9 15:43 manage.py*
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321    77 Sep  9 15:43 manage.sh*
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321   256 Sep  9 15:43 monitoring-cron
drwxrwxr-x 5 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 my_geonode/           <------- Django main app folder
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  3308 Sep  9 15:43 .override_dev_env.sample
drwxrwxr-x 3 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 package/
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 41964 Sep  9 15:43 pavement.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321    40 Sep  9 15:43 paver_dev.sh.sample
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321    31 Sep  9 15:43 paver.sh*
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  1566 Sep  9 15:43 playbook.yml
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  6173 Sep  9 15:43 README.md
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321    80 Sep  9 15:43 requirements.txt       <------- Django dependencies
drwxrwxr-x 3 geonode-vm-321 geonode-vm-321  4096 Sep  9 15:43 scripts/
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  1648 Sep  9 15:43 setup.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 22993 Sep  9 15:43 tasks.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321  2445 Sep  9 15:43 uwsgi.ini
-rwxr-xr-x 1 geonode-vm-321 geonode-vm-321   691 Sep  9 15:43 wait-for-databases.sh*
```

By taking a look into the `/opt/geonode-project/my_geonode/my_geonode` you should be able to recognize a standard Django app structure

```shell
drwxrwxr-x 5 geonode-vm-321 geonode-vm-321 4096 Sep  9 15:43 ./
drwxrwxr-x 7 geonode-vm-321 geonode-vm-321 4096 Sep  9 15:43 ../
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1279 Sep  9 15:43 apps.py
drwxrwxr-x 2 geonode-vm-321 geonode-vm-321 4096 Sep  9 15:43 br/
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1356 Sep  9 15:43 celeryapp.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1042 Sep  9 15:43 __init__.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 5000 Sep  9 15:43 settings.py   <------- Django settings
drwxrwxr-x 6 geonode-vm-321 geonode-vm-321 4096 Sep  9 15:43 static/
drwxrwxr-x 2 geonode-vm-321 geonode-vm-321 4096 Sep  9 15:43 templates/    <------- HTML templates
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1234 Sep  9 15:43 urls.py       <------- main URL patterns
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1779 Sep  9 15:43 version.py
-rw-rw-r-- 1 geonode-vm-321 geonode-vm-321 1980 Sep  9 15:43 wsgi.py
```

Some files are still missing, like the **views**, **models** and **migrations**, since they are not needed by default.

## Start `my_geonode` in DEV Mode
- We need to install the dependencies first

```shell
# Let's install the GeoNode dependencies
pip install -r requirements.txt

# Let's install the current app in dev mode
pip install -e .
```

- The template install by default the most recent GeoNode release from **PyPi**; we want here to link our `my_geonode` to the `geonode` currently present on the local machine

```shell
# Let's remove the currently installed GeoNode
pip uninstall GeoNode

# Let's install the local GeoNode in development mode
pip install -e /opt/geonode
```

We are good to go; the dependencies should be updated also. Let's prepare and start the `my_geonode` project.

```shell

```

#### [Next Section: Link GeoNode to a geonode-project instance](GEONODE_PROJ_DEV.md)
