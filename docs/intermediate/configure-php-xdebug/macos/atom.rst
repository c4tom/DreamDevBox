:orphan:

.. include:: /_includes/all.rst
.. include:: /_includes/snippets/__ANNOUNCEMENTS__.rst

.. _configure_php_xdebug_mac_atom:

********************************
Docker on MacOS: Xdebug for Atom
********************************

.. note:: Docker on MacOS requires you to create a **host address alias** on your loopback device.

**Table of Contents**

.. contents:: :local:


Prerequisites
=============

Ensure you know how to customize ``php.ini`` values for the DreamDevBox and have a rough understanding
about common Xdebug options.

.. seealso::
   * :ref:`php_ini`
   * :ref:`configure_php_xdebug_options`

.. important::
   Ensure you have created an :ref:`howto_host_address_alias_on_mac` and
   ``10.254.254.254`` is aliased to your localhost.


Assumption
==========

For the sake of this example, we will assume the following settings and file system paths:

+------------------------------+------------------------------------------+
| Directory                    | Path                                     |
+==============================+==========================================+
| DreamDevBox git directory       | ``/home/cytopia/repo/dreamdevbox``          |
+------------------------------+------------------------------------------+
| :ref:`env_httpd_datadir`     | ``./data/www``                           |
+------------------------------+------------------------------------------+
| Resulting local project path | ``/home/cytopia/repo/dreamdevbox/data/www`` |
+------------------------------+------------------------------------------+
| Selected PHP version         | ``5.6``                                  |
+------------------------------+------------------------------------------+
| Host address alias           | ``10.254.254.254`` (see prerequisites)   |
+------------------------------+------------------------------------------+

The **Resulting local project path** is the path where all projects are stored locally on your
host operating system. No matter what this path is, the equivalent remote path (inside the Docker
container) is always ``/shared/httpd``.

.. important:: Remember this, when it comes to path mapping in your IDE/editor configuration.


Configuration
=============

Install php-debug for Atom
--------------------------

Ensure you have ``php-debug`` installed for Atom.

   .. seealso:: |ext_lnk_xdebug_ide_atom_php_debug|

Configure Atom
--------------

You will need to configure the path mapping in ``config.cson`` (Atom configuration file) or via UI:

   .. code-block:: js
      :caption: config.cson
      :emphasize-lines: 6

      "php-debug":
      {
         ServerPort: 9000
         PathMaps: [
           "remotepath;localpath"
           "/shared/httpd;/home/cytopia/repo/dreamdevbox/data/www"
         ]
      }

   .. important::
      Recall the path settings from the *Assumption* section and adjust if your configuration differs!

Configure php.ini
-----------------

.. note:: The following example show how to configure PHP Xdebug for PHP 5.6:

Create an ``xdebug.ini`` file (must end by ``.ini``):

   .. code-block:: bash

      # Navigate to the DreamDevBox git directory
      host> cd path/to/dreamdevbox

      # Navigate to PHP 5.6 ini configuration directory
      host> cd cfg/php-ini-5.6/

      # Create and open debug.ini file
      host> vi xdebug.ini

Copy/paste all of the following lines into the above created ``xdebug.ini`` file:

   .. code-block:: ini
      :caption: xdebug.ini
      :emphasize-lines: 7,8,11

      ; Defaults
      xdebug.default_enable=1
      xdebug.remote_enable=1
      xdebug.remote_port=9000

      ; The MacOS way
      xdebug.remote_connect_back=0
      xdebug.remote_host=10.254.254.254

      ; idekey value is specific to Atom
      xdebug.idekey=xdebug.atom

      ; Optional: Set to true to always auto-start xdebug
      xdebug.remote_autostart=false

.. important:: Ensure you have created a :ref:`howto_host_address_alias_on_mac` pointing to ``10.254.254.254`` as stated in the prerequisites section above!

.. note:: Host os and editor specific settings are highlighted in yellow and are worth googling to get a better understanding of the tools you use and to be more efficient at troubleshooting.


Restart the DreamDevBox
--------------------

Restarting the DreamDevBox is important in order for it to read the new PHP settings.
Note that the following example only starts up PHP, HTTPD and Bind.

.. code-block:: bash

   # Navigate to the DreamDevBox git directory
   host> cd path/to/dreamdevbox

   # Stop, remove stopped container and start
   host> docker-compose stop
   host> docker-compose rm
   host> docker-compose up php httpd bind


.. seealso:: :ref:`start_the_dreamdevbox_stop_and_restart` (Why do ``docker-compose rm``?)
