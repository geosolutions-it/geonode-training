# Upgrade GeoNode to Latest Version
This simple procedure allow us to get latest updates from the `upstream` repository of GeoNode.

```shell
workon my_geonode
cd /opt/geonode
```

```shell
git fetch --all --prune
git pull origin 3.3.x

From https://github.com/GeoNode/geonode
 * branch                3.3.x      -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 .clabot                                                |   3 ++-
 .env                                                   |   2 +-
 geonode/catalogue/metadataxsl/tests.py                 |  14 ++++++++++++
 geonode/catalogue/metadataxsl/views.py                 |  24 ++++++++++++++-----
 geonode/layers/models.py                               |  24 ++++++++++---------
 geonode/layers/views.py                                |  14 ++++--------
 geonode/people/templates/people/profile_detail.html    |   2 +-
 geonode/services/migrations/0046_auto_20210903_1427.py |  77 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 geonode/services/models.py                             |  29 +++++++++++++++++++----
 geonode/services/serviceprocessors/arcgis.py           |  26 ++++++++++-----------
 geonode/services/serviceprocessors/base.py             |   2 +-
 geonode/services/serviceprocessors/wms.py              | 113 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++----------------------------------
 geonode/services/tasks.py                              |  17 ++++++++------
 geonode/services/tests.py                              |  66 ++++++++++++++++++++++++++++++++++++++++++++--------
 geonode/services/views.py                              |  15 ++++++------
 geonode/templates/500.html                             |   2 +-
 geonode/templates/base.html                            |  16 +++++++------
 geonode/thumbs/tests/test_integration.py               |  47 ++++++++++++++++++--------------------
 geonode/thumbs/tests/test_unit.py                      |   2 +-
 geonode/utils.py                                       |   2 +-
 requirements.txt                                       |   2 +-
 setup.cfg                                              |   2 +-
 22 files changed, 352 insertions(+), 149 deletions(-)
 create mode 100644 geonode/services/migrations/0046_auto_20210903_1427.py
```

```shell
pip install -r requirements.txt --upgrade
pip install -e . --upgrade
```

```shell
git push <fork name> <branch name>   <-- e.g.: git push afabiani geonode_training_32x
```

```shell
cd /opt/geonode-project/my_geonode/
./manage_prod.sh makemigrations
./manage_prod.sh migrate
./manage_prod.sh collectstatic
```

```shell
sudo pkill -9 -f uwsgi
sudo ps aux | grep uwsgi

sudo /etc/init.d/uwsgi restart
sudo ps aux | grep uwsgi
```

![image](https://user-images.githubusercontent.com/1278021/133283292-5dc34fb6-bef9-4815-bdc1-ebf27418f47d.png)
