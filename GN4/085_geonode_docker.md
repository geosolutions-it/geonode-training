# GeoNode and Docker

GeoNode needs a set of external services to run, including a DBMS (PostgreSQL), a message broker (rabbitMQ), a GeoServer instance. In order to develop, run and test GeoNode you probably will need one or more of these services up and running. 

As a developer, or as a frontend designer, you may want to concentrate only on your tasks, that will probably be a customization of a geonode project, without having to deal with all these sort of services.

Here comes handy Docker. Docker is a platform that enables you to separate your applications from your infrastructure. This means that by using Docker you can run all these ancillary services needed to GeoNode without installing them directly on your system, but having them run by docker in a sandboxed environment.

## Quick introduction to Docker

Docker runs services in *containers*, usually one container for each needed service.

You can see these containers as _lightweight virtual machines_: all of the files, binaries, libraries, configuration and so on, all lie within the container, so that your system libraries may differ from the ones in the container.

### Volumes

These containers are created and destroyed on request. The filesystem(s) they use may be as **volatile** as the container itself (when the container is destroyed the file system content is lost), or can be a **persistent** volume. This may be useful for instance if you run a DB in a container; when the service/container is restarted, the storage files are preserved, so you get back all the contents you had when the database was previously run. Persisted volumes are handled by the docker engine and may not be accessible from the outside of the container.

Another choice is having directories inside the container as **mapped volumes** onto local directories in the host filesystem; in this case any change in your host filesystem is reflected in the filesystem used by the container, and vice versa. This is an optimal solutions for developers: while working on the project, they only have to edit and save their part of the geonode project, and it will be immediately ready and used by geonode (in some case a restart of the container is required, but you got the point.)

## Docker compose

The `docker` command allows you to manage a single docker item at a time: container, images, volumes, etc.

In a generic architecture, and in GeoNode as a particular case, we need more than one service running. In order to orchestrate and handle a set of services `docker-compose` will be used.

`docker-compose` allows you to specify how different containers shall relate each other, also setting dependencies between containers (e.g.: "service X will not be started until service Y is up"). You can easily share volumes across different containers, a private network will be connecting the different containers.

# Files involved

The main configuration file for `docker-compose` is called `docker-compose.yml`.
Having created your GeoNode project from the `geonode-project` template you should already have that file in your project, perfectly ready for running your GeoNode instance and all of its required ancillary services.

You can configure the whole set of services, most of which share a subset of configuration items, by editing the file `.env`.

When running `docker-compose`, it will automatically (if not told otherwise) look for the files in the current directory:
- `.env`: environment variables
- `docker-compose.yml`: configuration for building and running the containers
- `docker-compose.override.yml`: local overriding of the containers configuration.

Our `docker-compose` will handle these services:
- `db`: a postgres instance
- `rabbitmq`: the rabbitmq service, used to control background processes
- `geoserver`: no further explanation needed for it :)
- `django`: the GeoNode "core" process
- `celery`: the service spawning background processes. 
  This container uses the very same image as the `django` container, since we have GeoNode code running in here as well.
- `geonode`: nginx service, which routes/proxies the requests toward geonode and geoserver.
- `letsencrypt`: used to manage certificates for the HTTPS protocol
- `data-dir-conf`: a service that initializes the GeoServer data dir

# Running docker

In your project directory, run:

```shell
docker-compose build
```

**Notice**: this command will last quite long (10-20 mins) and will download lots of stuff.

This command will build the _images_ for the _containers_. It's more or less like installing virtual machines from scratch, one for each service. You can think the **image** as a frozen installed VM, while the **container** is a living instance.

Once the images are built, you can run all the containers using the command:

```shell
docker-compose up
```

This will create all the needed volumes, create the virtual network, istantiate the images by running all the containers.

The processes will be run in the current shell and you will see the logs for all the containers scroll on the terminal.

If you want to detach the processes and make them run in background, you can use the `-d` argument (`docker-compose up -d`).

If you want to visualise the logs you can run the command:
```shell
docker-compose logs [-f] [container ...]
```
- `-f`: _follow_, works as `tail -f`
- `container`: name of the containers you want the logs of. If omitted, you'll get the logs for all the running containers.

### Stopping and restarting containers

You can stop the containers with the command `docker-compose stop` and restart with `docker-compose start`. Using these commands the volatile filesystem will be preserved between runs. You can also stop the containers using `docker-compose down`; this will destroy the current container and its volatile filesystem. You'll need to issue `up` again to recreate a fresh container from its image. 
 
#### [Next Section: Put geonode-project in Production](090_GEONODE_PROJ_PROD.md)