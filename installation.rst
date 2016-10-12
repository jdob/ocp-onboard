Installation
============

While there are a number of ways to `install the upstream OpenShift Origin <https://install.openshift.com/>`_
releases, the recommended way to install the Red Hat OpenShift Container
Platform is through the Red Hat Container Development Kit (CDK).

The CDK is a pre-built environment for running and developing containers using
Red Hat OpenShift. It runs as a virtual machine and supports a number of
different virtualization providers and host operating systems.

Developers can get the Red Hat Container Development Kit via the no-cost
Red Hat Enterprise Linux Developer Suite subscription, accessed through
http://developers.redhat.com/. Once registered, there are two pieces
that must be downloaded, with both being found at
http://developers.redhat.com/products/cdk/download/.

* The Red Hat Container Tools are used in conjunction with
  `Vagrant <https://www.vagrantup.com/>`_ to start the pre-built VM images.
  These tools include the necessary vagrant files and plugins for starting a VM
  running either Red Hat OpenShift or upstream Kubernates on its own,
  as well as registering the VM with RHN.
* The virtual machine image appropriate for your preferred hypervisor
  (the currently supported hypervisors include VirtualBox, libvirt,
  and HyperV.

Details on the CDK installation process can be found on the
`Red Hat Customer Portal <https://access.redhat.com/documentation/en/red-hat-container-development-kit/2.2/paged/installation-guide>`_.
Additional information on interacting with the CDK VMs can be found at
the `Vagrant Documentation <https://www.vagrantup.com/docs/>`_.
