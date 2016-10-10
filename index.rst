Partner Onboarding Guide
====================================================

Contents:

.. toctree::
   :maxdepth: 2

.. The scope of all of this (currently) is for containerized partners, not
   infra or SaaS related

.. The examples cited are going to be a simplistic Python application
   that responds to HTTP requests. I'm hoping I can use the same example in all
   cases and show it being built up from (minimal) code to all of the working
   OpenShift features. The context is that it's a running service
   that fields requests, but without assuming any knowledge or black magic from
   other libraries.

.. Initial informal landing page for partners of all levels of experience.
   Will introduce the fundamental OpenShift concepts (I'm a huge fan of glossaries)
   A few "If you want to..." guides to point at specifics depending on what it
     is the partner already has; these link into the pages referenced below, but
     are a friendlier avenue to them
   A list of useful things for devs to know (potentially debugging topics)

* Getting Started
 * Terminology (service, route, etc.)
 * Installing the CDK (overview and links)
 * If you have a working Docker container
  * Deploying from Docker Hub
   * Example
  * Building in OpenShift from a Dockerfile
   * Example
 * If you are developing a new application on OpenShift
  * Builder images
   * Example (show Python base image info)
  * S2I
   * Example
 * Useful Techniques (debugging?)
  * Remote shell to a container
  * Copying files into a running container

.. Starting point for devs who are using Docker already and want to quickly
   get those apps into OpenShift. I'm not in love with the router stuff being
   here, but that's also not "advanced" either

* Existing Docker Applications
 * Deploying from Docker Hub
 * Building a Dockerfile in OpenShift
 * Exposing your application

.. Starting point for devs who haven't actually migrated to Docker yet, so this
   will guide them on using OpenShift development using s2i. This also needs to
   talk about routes, so the exposing link will be the same as above.

* New Development
 * Understanding builder images
 * S2I
 * Exposing your application

.. I need a better title for this, but this is how to add OpenShift-ness to their
   application: using environment variables to make their containers configurable,
   creating templates to ease deployments, creating a builder image that they want
   users to build from (is this needed? sounds like something that might be useful),
   and how to add health checks to their container

* Modifying an application to be more OpenShift-friendly
 * Environment variables
  * Example
 * Adding a template for deploying your application
  * Example
 * Creating a builder image
  * Example
 * Liveness and Readiness
  * Example

.. I don't like the term "advanced", but it's a bit of a catch-all for things that
   I wouldn't consider part of every experience. These won't be copy/paste from the
   OpenShift developer guide, but rather trimmed down versions to inform partners
   of their existence and usefulness. References to the appropriate sections in the
   developer guide will be provided.

* Advanced Topics
 * Daemonsets
 * ConfigMaps
 * Secrets

