Basic Usage
===========

.. _resource_query_commands:

Resource Query Commands
-----------------------

The CLI provides two useful commands for listing and inspecting resources
in the OpenShift installation.

Get
~~~

The ``get`` command displays a list of a particular resource type, along with
some basic summary information on each entry. The desired resource type is
specified as an argument to the ``get`` call and the output will vary based
on the type of resource being queried. The full list of resource types can
be found by calling ``oc get`` with no arguments.

For example, retrieving the list of :term:`image stream` resources will display
tagging and repository information::

  $ oc get is
  NAME         DOCKER REPO                                TAGS      UPDATED
  python-web   172.30.53.244:5000/python-web/python-web   latest    2 hours ago

Retrieving the list of services, however, displays information on the
associated IP addresses and ports::

  $ oc get services                                                                                                                                                                  1 ↵
  NAME         CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
  python-web   172.30.167.215   <none>        8080/TCP   2h

Describe
~~~~~~~~

The ``describe`` command is used to retrieve specific details on a particular
resource. In addition to the resource's name, the type of resource must also
be provided (in many cases, resources of different types will share the same
name to ease understanding their relationship). If the resource name is
omitted, details about all resources of that type are displayed (depending
on your environment, the output may be very long and unwieldy).

Using the image stream found above, detailed information can be displayed
using::

  $ oc describe is python-web
  Name:                   python-web
  Namespace:              python-web
  Created:                2 hours ago
  Labels:                 app=python-web
  Annotations:            openshift.io/generated-by=OpenShiftNewApp
                          openshift.io/image.dockerRepositoryCheck=2016-10-14T15:05:43Z
  Docker Pull Spec:       172.30.53.244:5000/python-web/python-web
  Unique Images:          1
  Tags:                   1

  latest
    tagged from jdob/python-web

  * jdob/python-web@sha256:3f87be1825405ee8c7da23d7a6916090ecbb2d6e7b04fcd0fd1dc194173d2bc0
      2 hours ago

As with ``get``, the output will vary based on the type of resource being
described.

.. _routes:

Routes
------

When a container is created in OpenShift, it is initially assigned an IP
address and an internal service name within the scope of its project. The
service name allows it to be accessed by other applications running inside
of the same project. This becomes a useful default for large projects that
have a number of internal services but only a small amount of public
endpoints.

An explicit step is required to make a container publicly accessible. The
application must be `exposed` by creating a route. When a route is exposed,
the host name can be specified. In most cases, the DNS resolution for the
hostname is handled outside of OpenShift. If a hostname is not provided,
OpenShift will generate an xip.io address that can be used local to the
OpenShift instance.

By default, `HAProxy <http://www.haproxy.org/>`_ is used to manage the routes,
however `plugins for other providers are available <https://docs.openshift.org/latest/install_config/router/index.html#install-config-router-overview>`_.
Routes may optionally be configured with TLS credentials for secure
communications.

Routes are created through the ``expose`` command. Arguments are supported for
customizing the route (the most common being ``--hostname`` when using an
existing DNS server), but for development purposes, the defaults are usually
sufficient::

  $ oc expose service python-web
  route "python-web" exposed

  $ oc get route
  NAME         HOST/PORT                                    PATH      SERVICES     PORT       TERMINATION
  python-web   python-web-python-web.apps.10.2.2.2.xip.io             python-web   8080-tcp

  $ oc describe route
  Name:           python-web
  Namespace:      python-web
  Created:        6 seconds ago
  Labels:         app=python-web
  Annotations:    openshift.io/host.generated=true
  Requested Host: python-web-python-web.apps.10.2.2.2.xip.io exposed on router router 6 seconds ago

  Path:            <none>
  TLS Termination: <none>
  Insecure Policy: <none>
  Endpoint Port:   8080-tcp

  Service:   python-web
  Weight:    100 (100%)
  Endpoints: 172.17.0.12:8080

In the above example, OpenShift generated a corresponding xip.io address that
can be used to access the service. A quick test from the host running the
OpenShift VM shows the service can be accessed::

  $ curl python-web-python-web.apps.10.2.2.2.xip.io
  Hello World

More information on routes can be found in the
`corresponding section <https://docs.openshift.org/latest/architecture/core_concepts/routes.html>`_
of the OpenShift documentation.

Persistent Storage
------------------

Along with routes, a common configuration made to pods is the addition of
persistent storage volumes. As this is a lengthy topic, it can be found in
the :doc:`storage` section of this guide.

.. _remote_shell:

Remote Shell into a Container
-----------------------------

While debugging an application, there are many times where it can be useful
open a terminal into a running container. Both the web UI and command line
interface support this directly; there is no need to look up IP addresses
or manually deal with SSH keys.

The ``rsh`` command is used to connect to a specific pod by its name. The
pod names can be retrieved using the ``get`` command under the ``pods``
resource type::

  $ oc get pods
  NAME                 READY     STATUS    RESTARTS   AGE
  python-web-4-hwwub   1/1       Running   1          6d

  $ oc rsh python-web-4-hwwub
  # ls
  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  src  srv  sys  tmp  usr  var

Copying Files into a Running Container
--------------------------------------

Similar to opening a remote shell, the OpenShift interfaces provide built-in
support for copying files from a local system into a running container.

.. warning::

  Keep in mind that containers are typically treated as ephemeral. Files
  copied to a running container in this fashion are not guaranteed to survive
  pod restarts or scaling operations. This functionality is primarily intended
  for debugging or development situations.

Apropos of its name, the ``rsync`` command will attempt to use rsync to
transmit files if the service is available on the destination container. If
it is not, OpenShift will fall back to sending a tarfile with the contents.
Keep in mind that the normal restrictions when using tar over rsync will
be present; the destination directory must exist and the entire contents will
be transmitted rather than only sending changed files.

The ``rsync`` command works similar to the tranditional rsync command,
accepting the source and destination directories. Instead of specifying a
hostname or IP address, the pod name is used in the destination::

  $ oc get pods
  NAME                 READY     STATUS    RESTARTS   AGE
  python-web-4-hwwub   1/1       Running   1          6d

  $ oc rsync . python-web-4-hwwub:/doc
  WARNING: cannot use rsync: rsync not available in container
  [output truncated]

  $ oc rsh python-web-4-hwwub
  # ls /doc
  Makefile  README.md  _build  advanced.rst  basic-usage.rst

.. note::

  In the above example, rsync was not supported by the container, so the
  ``/doc`` directory was manually created in advance.
