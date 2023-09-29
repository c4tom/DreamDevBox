.. include:: /_includes/snippets/__ANNOUNCEMENTS__.rst

.. _remove_the_dreamdevbox:

*******************
Remove the DreamDevBox
*******************

If you want to completely remove the DreamDevBox follow this tutorial.


**Table of Contents**

.. contents:: :local:


Backups
=======

Before deleting the DreamDevBox git directory, ask yourself if you want to make backups of all
customizations you have done so far as well as of all data that may be present within that
directory.

Dump databases
--------------

Before shutting down the DreamDevBox, do a final backup of all of your databases:

.. seealso::

   * :ref:`backup_and_restore_mysql`
   * :ref:`backup_and_restore_pgsql`
   * :ref:`backup_and_restore_mongo`

Dumps will end up in ``backups/``.

Shutdown the DreamDevBox
---------------------

Before attempting to backup any file system data, make sure the DreamDevBox is properly shutdown.

.. code-block:: bash

   host> docker-compose stop

Backup configuration files
--------------------------

You should now backup the following configuration files:

* Backup your customized ``.env`` file
* Backup your customized ``.docker-compose.override.yml`` file
* Backup your customized bash configuration from ``bash/``
* Backup all custom service configurations from ``cfg/``
* Backup the DreamDevBox root certificate from ``ca/``

Backup data and dumps
---------------------

You should now backup the following data:

* Backup any backups created in ``backups/``
* Backup any project or Docker data from ``data/``


Remove the DreamDevBox
===================

If you have followed the backup routine, you can continue deleting all created components.

Remove DreamDevBox containers
--------------------------

Navigate to the DreamDevBox git directory and remove all DreamDevBox container:

   .. code-block:: bash

      host> docker-compose rm -f

Remove DreamDevBox network
-----------------------

1. List all existing Docker networks via

   .. code-block:: bash

      host> docker network ls

      NETWORK ID          NAME                                 DRIVER              SCOPE
      0069843ff0c3        bridge                               bridge              local
      ...
      9c8d4a84cf2d        dreamdevbox_app_net                     bridge              local

2. Find the NETWORK ID of the DreamDevBox network and delete it:

   .. code-block:: bash

      host> docker network rm 9c8d4a84cf2d

Remove DreamDevBox git directory
-----------------------------

You can simply delete the whole DreamDevBox git directory


Revert your system changes
==========================

AutoDNS
-------

Revert any changes you have done for Auto-DNS to work.

.. seealso:: :ref:`setup_auto_dns`

Manual DNS entries
------------------

Revert any changes you have done in ``/etc/hosts`` (or ``C:\Windows\System32\drivers\etc`` for Windows)

.. seealso::

   * :ref:`howto_add_project_hosts_entry_on_mac`
   * :ref:`howto_add_project_hosts_entry_on_win`

Remove DreamDevBox CA from your browser
------------------------------------

Remove the DreamDevBox CA from your browser

.. seealso::

   * :ref:`setup_valid_https`
