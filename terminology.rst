Terminology
===========

.. Currently, these are organized in a flow from the most high-level concepts
   to lower ones, but I may reorder this alphabetically in the future.

.. glossary::

  project
    An OpenShift project corresponds to a Kubernetes *namespace*. They are
    used to organize and group objects in the system, such as services and
    deployment configurations, as well as provide security policies specific
    to those resources.

  container
    The fundamental piece of an OpenShift application is a container. A
    container is a way to isolate and limit process interactions with minimal
    overhead and footprint. In most cases, a container will be limited to
    a single process providing a specific service (e.g. web server, database).

  pod
    Pods come from the Kubernetes concept of the same name. A pod is a set of
    one or more containers deployed together to act as if they are on a single
    host, sharing an internal IP, ports, and local storage.

    It is important to realize that OpenShift treats pods as immutable. Any
    changes, be it the underlying image, pod configuration, or environment
    variable values, cause new pods to be created and phase out the existing
    pods. Being immutable also means that any state is not maintained between
    pods when they are recreated.

  image
    An image is a pre-built, binary file that contains all of the necessary
    components to run a single container; a container is the working
    instantiation of an image. Additionally, an image defines certain
    information on how to interact with containers created from the image,
    such as what ports are exposed by the container.

    OpenShift uses the same image format as Docker; existing Docker images
    can easily be used to build containers through OpenShift. Additionally,
    OpenShift provides a number of ways to build images, either from a
    Dockerfile or directly from source hosted in a git repository.

  image stream
    An image stream is a series of Docker images identified by one or more
    tags. Image streams are capable of aggregating images from a variety
    of sources into a single view, including

    * Images stored in OpenShift's integrated Docker repository
    * Images from external Docker registries
    * Other image streams

  replication controller
    A replication controller is used to ensure a specified number of
    pods for an application are running at a given time. The replication
    controller automatically reacts to changes to deployed pods, both the
    removal of existing pods (deletion, crashing, etc.) or the addition of
    extra pods that are not desired. The pods are automatically added or
    removed from the service to ensure its uptime.

  deployment configuration
    A deployment configuration contains the details of a particular application
    deployment:

    * The configuration used in the :term:`replication controller` definition,
      such as the number of replicas to ensure
    * Triggers for automatically performing an updated deployment, such as
      when an image is tagged or the source code in a :term:`source-to-image`
      deployment is changed
    * The strategy for transitioning between deployments when upgrading
    * Lifecycle hooks

  service
    A service functions as a load balancer and proxy to underlying pods.
    Services are assigned IP addresses and ports and will delegate requests
    to an appropriate pod that can field it.

  route
    TODO

  source-to-image
    TODO
