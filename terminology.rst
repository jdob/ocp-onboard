Terminology
===========

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
