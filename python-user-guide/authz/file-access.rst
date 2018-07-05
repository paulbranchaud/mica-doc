File Access
===========

This command is used to manage the access to a file in the Mica file system. This access affects the **published** version.

.. code-block:: bash

  mica access-file PATH <CREDENTIALS> [OPTIONS] [EXTRAS]

Arguments
---------

============ ===========
Argument     Description
============ ===========
``PATH``     Path to the file in the Mica file system
============ ===========

Options
-------

================================================= ====================================
Option                                            Description
================================================= ====================================
``--add, -a``                                     Add an access
``--delete, -d``                                  Delete an access
``--subject, -s``                                 Subject name to which the access will be granted
``--type TYPE, -ty TYPE``                         Subject type: ``user`` or ``group``
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

Add access for user demouser on demo individual-study files:

.. code-block:: bash

  mica access-file /individual-study/demo --mica http://mica-demo.obiba.org --user administrator --password password --type USER --subject demouser --add

Remove the above access:

.. code-block:: bash

  mica access-file /individual-study/demo --mica http://mica-demo.obiba.org --user administrator --password password --type USER --subject demouser --delete
