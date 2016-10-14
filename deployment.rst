Deploying or Building an Existing Container
===========================================

.. Starting point for devs who are using Docker already and want to quickly
   get those apps into OpenShift. I'm not in love with the router stuff being
   here, but that's also not "advanced" either

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

.. warning::
  By default, the newly deployed application will not be accessible. A
  :term:`route` is needed to expose the service to the outside world. See
  the :ref:`routes` section for more information on how to expose and
  access a service.

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
