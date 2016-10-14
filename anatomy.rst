Anatomy of a Project
--------------------

.. note::

  More information on the ``get`` and ``describe`` commands that are used in
  this guide can be found in the :ref:`resource_query_commands` section.

When applications are deployed, either directly from an image or built using
:term:`source-to-image`, there are a number of resources that OpenShift creates
to support the application.

Image Stream
~~~~~~~~~~~~

If an existing :term:`image stream` is not found, OpenShift will create one.
The image stream is used to provide images when creating new containers.
Additionally, triggers can be created to automatically react to changes in
an image stream contents and roll out updated pods.

The type name ``imagestreams`` (or ``is`` for short) is used with CLI query
commands::

  $ oc get is
  NAME         DOCKER REPO                                TAGS      UPDATED
  python-web   172.30.53.244:5000/python-web/python-web   latest    50 minutes ago

In the output above, the ``172.30.53.244`` address corresponds to the internal
Docker registry created and managed by the OpenShift installation. It runs
in a container under the ``default`` project, which can be accessed by a user
with cluster admin privileges::

  $ oc project default
  Now using project "default" on server "https://localhost:8443".

  $ oc get services
  NAME              CLUSTER-IP       EXTERNAL-IP   PORT(S)                   AGE
  docker-registry   172.30.53.244    <none>        5000/TCP                  20d
  kubernetes        172.30.0.1       <none>        443/TCP,53/UDP,53/TCP     20d
  router            172.30.198.186   <none>        80/TCP,443/TCP,1936/TCP   20d

More information about the created input stream can be viewed using the
``describe`` command::

  $ oc describe is python-web
  Name:             python-web
  Namespace:        python-web
  Created:          56 minutes ago
  Labels:           app=python-web
  Annotations:		openshift.io/generated-by=OpenShiftNewApp
                    openshift.io/image.dockerRepositoryCheck=2016-10-14T15:05:43Z
  Docker Pull Spec: 172.30.53.244:5000/python-web/python-web
  Unique Images:    1
  Tags:             1

  latest
    tagged from jdob/python-web

    * jdob/python-web@sha256:3f87be1825405ee8c7da23d7a6916090ecbb2d6e7b04fcd0fd1dc194173d2bc0
        56 minutes ago


Replication Controller
~~~~~~~~~~~~~~~~~~~~~~

A :term:`replication controller` is created when an application is deployed
and is used to control the number of running pods. Each application deployment
gets its own replication controller.

The resource type ``replicationcontrollers`` (or ``rc`` for short) is used
with the CLI query commands::

  $ oc get rc
  NAME           DESIRED   CURRENT   AGE
  python-web-1   1         1         1h

The ``describe`` command displays extra information, including details on the
image used to provision and details on the running and desired pods::

  $ oc describe rc python-web
  Name:           python-web-1
  Namespace:      python-web
  Image(s):       jdob/python-web@sha256:3f87be1825405ee8c7da23d7a6916090ecbb2d6e7b04fcd0fd1dc194173d2bc0
  Selector:       app=python-web,deployment=python-web-1,deploymentconfig=python-web
  Labels:         app=python-web
                  openshift.io/deployment-config.name=python-web
  Replicas:       1 current / 1 desired
  Pods Status:    1 Running / 0 Waiting / 0 Succeeded / 0 Failed
  No volumes.
  No events.


Deployment Configuration
~~~~~~~~~~~~~~~~~~~~~~~~

The next level up is the :term:`deployment configuration` which describes
when and how deployments of the application will be run.

The resource type ``deploymentconfigs`` (or ``dc`` for short) is used with
the CLI query commands::

  $ oc get dc
  NAME         REVISION   DESIRED   CURRENT   TRIGGERED BY
  python-web   1          1         1         config,image(python-web:latest)

Information on the deployment's triggers and update strategy, as well as
details on deployments done using the configuration, are displayed by the
``describe`` command::

  $ oc describe dc
  Name:           python-web
  Namespace:      python-web
  Created:        2 hours ago
  Labels:         app=python-web
  Annotations:    openshift.io/generated-by=OpenShiftNewApp
  Latest Version: 1
  Selector:       app=python-web,deploymentconfig=python-web
  Replicas:       1
  Triggers:       Config, Image(python-web@latest, auto=true)
  Strategy:       Rolling
  Template:
    Labels:       app=python-web
                  deploymentconfig=python-web
    Annotations:  openshift.io/container.python-web.image.entrypoint=["/bin/sh","-c","cd /src/www; /bin/bash -c 'python3 -u /src/web.py'"]
                  openshift.io/generated-by=OpenShiftNewApp
    Containers:
     python-web:
      Image:                      jdob/python-web@sha256:3f87be1825405ee8c7da23d7a6916090ecbb2d6e7b04fcd0fd1dc194173d2bc0
      Port:                       8080/TCP
      Volume Mounts:              <none>
      Environment Variables:      <none>
    No volumes.

  Deployment #1 (latest):
        Name:           python-web-1
        Created:        2 hours ago
        Status:         Complete
        Replicas:       1 current / 1 desired
        Selector:       app=python-web,deployment=python-web-1,deploymentconfig=python-web
        Labels:         app=python-web,openshift.io/deployment-config.name=python-web
        Pods Status:    1 Running / 0 Waiting / 0 Succeeded / 0 Failed


Service
~~~~~~~

The last layer of interest is the created :term:`service`. The service acts
as the entry point into the running application, taking care of distributing
requests to the appropriate pod.

The resource type ``services`` is used with the CLI query commands::

  $ oc get services
  NAME         CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
  python-web   172.30.167.215   <none>        8080/TCP   1h

Details about a service include the internal IP address and ports in use::

  $ oc describe service python-web
  Name:             python-web
  Namespace:        python-web
  Labels:           app=python-web
  Selector:         app=python-web,deploymentconfig=python-web
  Type:             ClusterIP
  IP:               172.30.167.215
  Port    :         8080-tcp    8080/TCP
  Endpoints:        172.17.0.12:8080
  Session Affinity:	None
  No events.

