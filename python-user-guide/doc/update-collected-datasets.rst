Update Collected Datasets
=========================

This command is for updating and/or publishing a list Collected Datasets which are ID is filtered by a `regular expression <https://docs.python.org/2/library/re.html>`_. The goal is to automate the linkage between a table in Opal with a collected dataset in Mica.

.. code-block:: bash

  mica update-collected-datasets ID <CREDENTIALS> [OPTIONS] [EXTRA]

Arguments
---------

============ ===========
Argument     Description
============ ===========
``ID``       A `regular expression <https://docs.python.org/2/library/re.html>`_ to filter the collected dataset identifiers.
============ ===========

Options
-------

================================================= ====================================
Option                                            Description
================================================= ====================================
``--project PROJECT, -prj PROJECT``               The associated Opal project.
``--dry DRY, -d DRY``                             Dry run of the command to list the collected datasets matching the regular expression.
``--publish, -pu``                                Publish the collected datasets.
``--unpublish, -un``                              Unpublish the collected datasets.
================================================= ====================================

Credentials
-----------

Authentication is done by username/password credentials.

==================================== ====================================
Option                               Description
==================================== ====================================
``--mica MICA, -mk MICA``            Mica server base url.
``--user USER, -u USER``             User name. User with appropriate permissions is expected depending of the REST resource requested.
``--password PASSWORD, -p PASSWORD`` User password.
==================================== ====================================

Extras
------

================= =================
Option            Description
================= =================
``-h, --help``    Show the command help's message
``--verbose, -v`` Verbose output
================= =================

Example
-------

Link the collected datasets which ID starts with 'cls-wave' in local Mica to a project in Opal and publish them.

.. code-block:: bash

  mica update-collected-datasets -u administrator -p password --project CLS --publish '^cls-wave'
