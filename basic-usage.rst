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

TODO

Remote Shell into a Container
-----------------------------

TODO

Copying Files into a Running Container
--------------------------------------

TODO


