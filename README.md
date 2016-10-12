# Building the Documentation

The documentation is written to be built using Sphinx. Sphinx can be installed
(preferably into a virtualenv) using pip:

    pip install sphinx

A makefile is included to facilitate the builds. Currently, HTML is the only
supported output::

    make html

The generated documentation will be found under the `_build/html` directory.

# How to Contribute

Any help is appreciated, be it clarification or additions on an existing
section or suggestions for new topics to cover. Simply fork the repository
into your account and submit pull requests against the original.
