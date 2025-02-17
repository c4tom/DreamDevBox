:orphan:

.. include:: /_includes/snippets/__ANNOUNCEMENTS__.rst

.. _howto_move_backups_to_a_different_directory:

*************************************
Move backups to a different directory
*************************************

No matter if your backups are already in a different location or if you want to move them out of
the DreamDevBox git directory now, you can do that in a few simple steps.

**Table of Contents**

.. contents:: :local:

Move backups out of the DreamDevBox git directory
==============================================

All you have to do is to adjust the path of :ref:`env_host_path_backupdir` in the ``.env`` file.

.. code-block:: bash

   # Navigate to DreamDevBox git directory
   host> cd path/to/dreamdevbox

   # Open the .env file with your favourite editor
   host> vim .env

Now Adjust the value of :ref:`env_host_path_backupdir`

.. code-block:: bash
   :caption: .env
   :emphasize-lines: 1

   HOST_PATH_HTTPD_DATADIR=/home/user/backups/dreamdevbox/

That's it, whenever you start up the DreamDevBox, ``/home/user/backups/dreamdevbox/`` will be mounted into
the PHP container into ``/shared/backups/``.
