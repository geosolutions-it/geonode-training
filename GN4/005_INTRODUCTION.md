# Introduction to GeoNode development 

This tutorial will introduce you the GeoNode architecture, on which framework it is based, and what can be built upon it.

GeoNode development can be split in two major purposes:

- GeoNode development itself, intended for improvements, bugs fixing and implementation of new features.

- Customization of GeoNode, related to modification of the model, customization of the look and feel, implementation of custom features.


## GeoNode development

This kind of development is usually done directly into the GeoNode core repository, according to the usual procedures that most of the Open Source communities follow (fork of the official repo, implementation in the fork, pull request back into the original repository).

Implementing customization inside the core GeoNode core is never a good option, because it will make it harder upgrading GeoNode; pulling official modifications into the customized code may in fact create code conflicts, that may need further work and that could be hard to solve.

In next chapters we'll see how to modify the code in the core GeoNode.  


## GeoNode customization

GeoNode is based on a Django, a Web framework that will be introduced and detailed in next chapter. Django organizes the code in various apps, and makes available ways to "connect" such apps.

Thanks to this way to organize code, you can create a customization for GeoNode as one (or more) django apps. In this way your customization will be decoupled from the GeoNode core code, allowing you to update GeoNode (for instance to import new fixes) without generating any code conflict. 

In next chapters we'll see how to create and maintain a GeoNode project starting from a provided template.


## Docker

GeoNode needs a set of external services to run, including a DBMS (PostgreSQL), a message broker (rabbitMQ), a GeoServer instance. In order to develop, run and test GeoNode you probably will need one or more of these services up and running. 

As a developer, or as a frontend designer, you may want to concentrate only on a single part of GeoNode, without having to deal with all these sort of services.

Here comes handy Docker. Docker is a platform that enables you to separate your applications from your infrastructure. This means that by using Docker you can run all these ancillary services needed to GeoNode without installing them directly on your system, but having them run by docker in a sandboxed environment.

In next chapters we'll see how to run a set of services handled by Docker.

#### [Next Section: Introduction to Django](010_DJANGO_INTRO.md)
