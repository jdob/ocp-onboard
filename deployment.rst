Building or Deploying an Existing Container
===========================================

OpenShift can deploy existing code in a number of ways. Pre-built containers
stored in a Docker registry, such as `Docker Hub <https://hub.docker.com/>`_
can be downloaded and deployed directly to OpenShift. OpenShift can also
build images from source code in a `git https://git-scm.com/`_ repository,
regardless of whether or not a Dockerfile is present.

Deploying from Docker Hub
-------------------------

The simplest way to get an existing image into OpenShift is to retrieve
the image from Docker Hub. OpenShift will automatically create a new
:term:`image stream` and map to the image in Docker Hub.

CLI Example
~~~~~~~~~~~

Create a new application in the current project, specifying the name of the
image in Docker Hub::

  $ oc new-app jdob/python-web

That's it. OpenShift will take care of retrieving the image and setting up
all of the necessary resources to deploy pods for the application. See
the :doc:`anatomy` section guide for more information on the different
resources that were created, or the :doc:`basic-usage` guide for other ways
to interact with the newly deployed application.

.. include:: includes/route-warning.txt

UI Example
~~~~~~~~~~

.. TODO: Add UI screenshots

Building a Dockerfile in OpenShift
----------------------------------

CLI Example
~~~~~~~~~~~

UI Example
~~~~~~~~~~

Next Steps
----------
