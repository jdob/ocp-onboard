Partner Onboarding Guide
========================

.. The scope of all of this (currently) is for containerized partners, not
   infra or SaaS related

.. The examples cited are going to be a simplistic Python application
   that responds to HTTP requests. I'm hoping I can use the same example in all
   cases and show it being built up from (minimal) code to all of the working
   OpenShift features. The context is that it's a running service
   that fields requests, but without assuming any knowledge or black magic from
   other libraries.

This guide is intended to help developers and integrators get started using
and developing for the OpenShift Container Platform.

Quick Start
-----------

:doc:`terminology` - If you're new to OpenShift, start here to get acquainted
with the common terms.

:doc:`installation` - If you don't have access to a running installation or
want to set up a local environment, check out the installation guide for more
information on installing the Red Hat Container Development Kit (CDK).

:doc:`deployment` - If you already have a container deployed to a public
registry (such as Docker Hub) or a Dockerfile, view the existing container
deployment guide.

:doc:`basic-usage` - Once you've got an application deployed, there are some
basic commands that can be run to understand all of its pieces and how to
interact with it.

:doc:`development` - If you're just beginning to port an application to
be run on containers, or beginning new development, the development guide
can provide more information on how to use OpenShift in an active development
environment.


Contents
--------

.. toctree::
   :maxdepth: 2

   terminology
   installation
   deployment
   basic-usage
   development
   anatomy
   advanced
