Installation
============

Mica is a stand-alone `Java <https://www.java.com>`_ server application that requires `MongoDB <https://www.mongodb.com/>`_ as database engine.

Requirements
------------

Server Hardware Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============ ===============
Component    Requirement
============ ===============
CPU	         Recent server-grade or high-end consumer-grade processor
Disk space	 8GB or more.
Memory (RAM) Minimum: 4GB, Recommended: >4GB
============ ===============

Server Software Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

======== ================= ========================================================== ========================
Software Suggested version Download link                                              Usage
======== ================= ========================================================== ========================
Java     >= 1.8.x          `Java Oracle downloads <https://www.java.com>`_            Java runtime environment
MongoDB  >= 2.4.x          `MongoDB downloads <http://www.mongodb.org/downloads>`_    Database engine
======== ================= ========================================================== ========================

While Java is required by Mica server application, MongoDB can be installed on another server.

Install
-------

Mica is distributed as a Debian/RPM package and as a zip file. The resulting installation has default configuration that makes Mica ready to be used (as soon as a MongoDB server is available). Once installation is done, see :doc:`configuration` instructions.

Debian Package Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mica is available as a Debian package from OBiBa Debian repository. To proceed installation, do as follows:

* `Install Debian package <http://www.obiba.org/pages/pkg/>`_. Follow the instructions in the repository main page for installing Mica.
* Manage Mica Service: after package installation, Mica server is running: see how to manage the Service.

RPM Package Installation
~~~~~~~~~~~~~~~~~~~~~~~~

Mica is available as a RPM package from OBiBa RPM repository. To proceed installation, do as follows:

* `Install RPM package <http://www.obiba.org/pages/rpm/>`_. Follow the instructions in the RPM repository main page for installing Mica.
* Manage Mica Service: after package installation, Mica is running: see how to manage the Service.

Zip Distribution Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mica is also available as a Zip file. To install Mica zip distribution, proceed as follows:

* `Download Mica distribution <https://github.com/obiba/mica2/releases>`_
* Unzip the Mica distribution. Note that the zip file contains a root directory named **mica-x.y.z-dist** (where x, y and z are the major, minor and micro releases, respectively). You can copy it wherever you want. You can also rename it.
* Create an ``MICA_HOME`` environment variable
* Separate Mica home from Mica distribution directories (recommended). This will facilitate subsequent upgrades.

Set-up example for Linux:

.. code-block:: bash

  mkdir mica-home
  cp -r mica-x-dist/conf mica-home
  export MICA_HOME=`pwd`/mica-home
  ./mica-x-dist/bin/mica

Launch Mica. This step will create/update the database schema for Mica and will start Mica: see Regular Command.

For the administrator accounts, the credentials are "administrator" as username and "password" as password. See User Directories Configuration to change it.

Upgrade
-------

The upgrade procedures are handled by the application itself.

Debian Package Upgrade
~~~~~~~~~~~~~~~~~~~~~~

If you installed Mica via the Debian package, you may update it using the command:

.. code-block:: bash

  apt-get install mica

RPM Package Upgrade
~~~~~~~~~~~~~~~~~~~

If you installed Mica via the RPM package, you may update it using the command:

.. code-block:: bash

  yum install mica

Zip Distribution Upgrade
~~~~~~~~~~~~~~~~~~~~~~~~

Follow the Installation of Mica Zip distribution above but make sure you don't overwrite your mica-home directory.


Upgrading to Mica 2.x
~~~~~~~~~~~~~~~~~~~~~

Mica 2.x is a major upgrade. Upgrade will affect the configuration files and database access.

.. warning::
  As a general rule, always backup the configuration files and the databases before upgrading. Make sure also you can restore them!

.. warning::
  To migrate to Mica 2.x, you need to have Mica >= 1.2.0

Mongodb access
++++++++++++++

For an automatic migration, Mica requires a user with full access to MongoDB. Make sure to create the role below and assign it to ``micaadmin``:

.. code-block:: javascript

  use admin
  db.createRole({
    role: 'obibauser',
    privileges:[{
      resource: {anyResource: true},
      actions: ['anyAction']
    }],
    roles: []
  });

  db.grantRolesToUser(
    "micaadmin",
    [ {role: "obibauser", db: "admin"} ]
  );

Mica configuration file
+++++++++++++++++++++++

Some configurations changed in file ``/etc/mica2/application.yml``

Configurations with prefix "mongodb." are useless, you can delete them. Now, we need the property "spring.data.mongodb.uri". Exemple for the above user:

.. code-block:: properties

  spring:
    data:
      mongodb:
        uri:
          mongodb://micaadmin:micaadmin@localhost:27017/mica?authSource=admin


Upgrading to Mica 3.x
~~~~~~~~~~~~~~~~~~~~~

This is a major upgrade consisting of several model changes and the addition of the new Harmonization Study document.

.. warning::

  To migrate to Mica 3.x, you need to first install 2.2.3, run the server once before you install the 3.x version.

In addition, the following taxonomy changes can affect the current installations. Before upgrading to Mica 3.x and to prevent conflicts or loss of
data make sure your taxonomy vocabularies do not match the following:

New Study taxonomy vocabularies:

* className: denoting the type of study document (Individual or Harmonization)
* harmonizationDesign
* populations-id
* populations-name
* populations-description
* populations-dataCollectionEvents-id
* populations-dataCollectionEvents-name
* populations-dataCollectionEvents-start
* populations-dataCollectionEvents-end
* populations-dataCollectionEvents-description

New variable taxonomy vocabularies:

* studyId
* populationId
* dceId

After upgrading to Mica 3.0 make sure to clear all cache and re-index all documents in the administration module of the Mica server.

.. note::

  A possible safeguard against taxonomy conflicts is to prefix custom vocabulary keys (omitting '.') so future updates do not override
  them.


Execution
---------

Server launch
~~~~~~~~~~~~~

**Service**

When Mica is installed through a Debian/RPM package, Mica server can be managed as a service.

Options for the Java Virtual Machine can be modified if Mica service needs more memory. To do this, modify the value of the environment variable ``JAVA_ARGS`` in the file **/etc/default/mica**.

Main actions on Mica service are: ``start``, ``stop``, ``status``, ``restart``. For more information about available actions on Mica service, type:

.. code-block:: bash

  service mica help

The Mica service log files are located in **/var/log/mica** directory.

**Manually**

The Mica server can be launched from the command line. The environment variable ``MICA_HOME`` needs to be setup before launching Mica manually.

==================== ======== ===========
Environment variable Required Description
==================== ======== ===========
``MICA_HOME``        yes      Path to the Mica "home" directory.
``JAVA_OPTS``        no       Options for the Java Virtual Machine. For example: `-Xmx4096m -XX:MaxPermSize=256m`
==================== ======== ===========

To change the defaults update:  ``bin/mica`` or ``bin/mica.bat``

Make sure Command Environment is setup and execute the command line (bin directory is in your execution PATH)):

.. code-block:: bash

  mica

Executing this command upgrades the Mica server and then launches it.

The Mica server log files are located in **MICA_HOME/logs** directory. If the logs directory does not exist, it will be created by Mica.

Usage
~~~~~

To access Mica with a web browser the following urls may be used (port numbers may be different depending on HTTP Server Configuration):

* http://localhost:8082 will provide a connection without encryption,
* https://localhost:8445 will provide a connection secured with ssl.

Troubleshooting
~~~~~~~~~~~~~~~

If you encounter an issue during the installation and you can't resolve it, please report it in our `Mica Issue Tracker <https://github.com/obiba/mica2/issues>`_.

Mica logs can be found in **/var/log/mica**. If the installation fails, always refer to this log when reporting an error.
