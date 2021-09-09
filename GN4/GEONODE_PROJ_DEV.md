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

#### [Next Section: Link GeoNode to a geonode-project instance](GEONODE_PROJ_DEV.md)
