Integrating with OpenShift
==========================

While it is simple to :doc:`deploy an image on OpenShift <deployment>`,
there are certain guidelines and integration techniques that should be
adhered to.


Creating Images
---------------

The OpenShift documentation contains a section on
`image creation guidelines <https://docs.openshift.com/container-platform/3.3/creating_images/guidelines.html>`_,
so rather than repeat that information here, it's recommended to be familiar
with the practices in that document. Points of particular interest will be
included in the rest of this guide.

Environment Variables
---------------------

It is impractical to rebuild an image for each possible configuration,
especially if the image is owned by someone else. The recommended mechanism
for configuring a service running in a container is through environment
variables.

Environment variables are set in the deployment configuration. They can be
set when the configuration is first created or can be added/modified/removed
after an application has been deployed. It is important to realize that
changes to the environment variables constitute a deployment configuration
change. The existing pods are not modified; rather, new pods are deployed with
the changed configuration and will automatically replace the existing ones
(this is easily seen on the UI overview page with the graphical depictions of
scaling up and down).

This paradigm is used both for images created for deployment as well as those
intended to be used as builder images with :ref:`build_s2i`. For example,
the Python builder image will, by default, attempt to run a file named
``app.py``. If an application cannot be modified to use this naming scheme,
the environment variable ``APP_FILE`` can be specified to indicate a new
script to run when the container is launched.

Example
~~~~~~~

Below is the output for the deployment configuration of a simple web
application. The application is written such that it will output "Hello
World" when it is not overridden with environment variables.

As a reminder, the list of a particular resource can be viewed using the
``get`` command, followed by the resource type in question (``dc`` is an
abbreviaton for deployment configuration):

.. code-block:: none

   $ oc get dc
   NAME         REVISION   DESIRED   CURRENT   TRIGGERED BY
   python-web   3          1         1         config,image(python-web:latest)

The details of the configuration can be displayed with the ``describe``
command (some of the irrelevant information has been removed for brevity):

.. code-block:: none
   :emphasize-lines: 22

   $ oc describe dc python-web
   Name:        python-web
   Namespace:   python-web
   Created:     10 days ago
   Labels:      app=python-web
   Annotations: openshift.io/generated-by=OpenShiftNewApp
   Latest Version:   3
   Selector:    app=python-web,deploymentconfig=python-web
   Replicas:    1
   Triggers:    Config, Image(python-web@latest, auto=true)
   Strategy:    Rolling
   Template:
     Labels:    app=python-web
       deploymentconfig=python-web
     Annotations:   openshift.io/container.python-web.image.entrypoint=["/bin/sh","-c","cd /src/www; /bin/bash -c 'python3 -u /src/web.py'"]
       openshift.io/generated-by=OpenShiftNewApp
     Containers:
       python-web:
         Image:                 jdob/python-web@sha256:3f87be1825405ee8c7da23d7a6916090ecbb2d6e7b04fcd0fd1dc194173d2bc0
         Port:                  8080/TCP
         Volume Mounts:         <none>
         Environment Variables: <none>
     No volumes.

Note that there are no environment variables set for the application. Viewing
the application (through its route), displays the default "Hello World" text:

.. code-block:: none

   $ oc get route
   NAME         HOST/PORT                                    PATH      SERVICES     PORT       TERMINATION
   python-web   python-web-python-web.apps.10.2.2.2.xip.io             python-web   8080-tcp

   $ curl http://python-web-python-web.apps.10.2.2.2.xip.io
   Hello World

There are a few options for editing environment variables. The UI can be
used to navigate to the deployment configuration. The "Environment" tab
can be used to view and modify environment variables for the configuration.
When changes are saved by pressing the "Save" button, a new deployment
is triggered using the new configuration values.

.. image:: images/env_variables.png

Alternatively, the CLI's ``edit`` command can be used to interactively edit
the YAML representation of many resources. This command, called by specifying
a resource type and name, opens a text editor in which changes can be made.
When the file is saved and the editor is closed, the changes are sent to
the server and the appropriate action is taken. In this case, the change in
configuration will cause a redeployment.

Below is a snippet of the deployment configuration while being edited
(removed sections are replaced with ``[snip]`` for readability). The
changes made are highlighted:

.. code-block:: none
   :emphasize-lines: 17-19

   $ oc edit dc python-web
   # Please edit the object below. Lines beginning with a '#' will be ignored,
   # and an empty file will abort the edit. If an error occurs while saving this file will be
   # reopened with the relevant failures.
   #
   apiVersion: v1
   kind: DeploymentConfig
   metadata:
     [snip]
   spec:
     [snip]
     template:
       metadata:
         [snip]
       spec:
         containers:
         - env:
           - name: TEXT
             value: Goodbye World
           image: jdob/python-web@sha256:3f87be1825405ee8c7da23d7a6916090ecbb2d6e7b04fcd0fd1dc194173d2bc0
           imagePullPolicy: Always
           name: python-web

The changes above introduce a new environment variable named ``TEXT`` which,
if present, is output by the web server when it is accessed. For reference,
the relevant Python line in the application is::

    m = os.environ.get('TEXT', None) or 'Hello World'

At this point, there are a few ways to monitor the changes being made. The
UI presents a graphical view of the existing pods scaling down while new ones
are created with the new configuration. The CLI's ``status`` command can be
used to show that a new deployment was made:

.. code-block:: none

   $ oc status                                                                                                                                                                        1 ↵
   In project python-web on server https://localhost:8443

   http://python-web-python-web.apps.10.2.2.2.xip.io to pod port 8080-tcp (svc/python-web)
     dc/python-web deploys istag/python-web:latest
       deployment #2 deployed 9 minutes ago - 1 pod
       deployment #1 deployed 48 minutes ago

Notice that a new deployment was made, corresponding to the updated deployment
configuration that was submitted. As proof of the environment variable's
presence in the container, the previous ``curl`` command can be run again:

.. code-block:: none

   $ curl http://python-web-python-web.apps.10.2.2.2.xip.io
   Goodbye World

Users
-----

Example
~~~~~~~

Labels
------

Example
~~~~~~~

Writing Deployment Templates
----------------------------



Example
~~~~~~~

.. _create_builder_image:

Creating a Builder Image
------------------------

Example
~~~~~~~

Liveness and Readiness
----------------------

Example
~~~~~~~
