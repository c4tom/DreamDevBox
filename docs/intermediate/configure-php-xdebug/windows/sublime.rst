:orphan:

.. include:: /_includes/all.rst
.. include:: /_includes/snippets/__ANNOUNCEMENTS__.rst

.. _configure_php_xdebug_win_sublime:

********************************************
Docker on Windows: Xdebug for Sublime Text 3
********************************************

Docker for Windows requires a well known IP address in order to connect to the host operating
system.

**Table of Contents**

.. contents:: :local:


Prerequisites
=============

General
-------

Ensure you know how to customize ``php.ini`` values for the DreamDevBox and have a rough understanding
about common Xdebug options.

.. seealso::
   * :ref:`php_ini`
   * :ref:`configure_php_xdebug_options`

Gather Host IP address
----------------------

On Windows you will have to manually retrieve the IP address to which Xdebug should connect to
via ``xdebug.remote_host``.

When you have done no custom configuration in your Virtual Switch manager, Docker for Windows will
use the ``Default Switch`` automatically.

   .. include:: /_includes/figures/xdebug/windows/virtual-switch-manager.rst

1. Open command line
2. Enter ``ipconfig``
3. Look for the IP4 address in ``Default Switch`` (e.g.: ``192.168.0.12``)

   .. include:: /_includes/figures/xdebug/windows/ipconfig.rst

.. important::
      ``192.168.0.12`` is meant as an example and will eventually differ on your system.
      Ensure you substitute it with the correct IP address.

.. seealso:: :ref:`howto_open_terminal_on_win`


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
| DockerNAT IP address         | ``192.168.0.12``                         |
+------------------------------+------------------------------------------+
| Virtual Switch IP address    | ``192.168.0.12``                         |
+------------------------------+------------------------------------------+

The **Resulting local project path** is the path where all projects are stored locally on your
host operating system. No matter what this path is, the equivalent remote path (inside the Docker
container) is always ``/shared/httpd``.

.. important:: Remember this, when it comes to path mapping in your IDE/editor configuration.


Configuration
=============

Install Xdebug Client for Sublime
---------------------------------

Use Sublime's Package Control to search for and install ``Xdebug Client``.

   .. seealso:: |ext_lnk_xdebug_ide_sublime_xdebug_client|

Configure Sublime
-----------------

* Navigate to ``Tools`` -> ``Xdebug`` -> ``Settings - User`` in the menu
* This will open the configuration file in Sublime

   .. code-block:: json
      :caption: Xdebug-sublime-settings
      :emphasize-lines: 3,6

      {
          "path_mapping": {
              "/shared/httpd" : "/home/cytopia/repo/dreamdevbox/data/www"
          },
          "url": "",
          "ide_key": "sublime.xdebug",
          "host": "0.0.0.0",
          "port": 9000
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

      ; The Windows way
      xdebug.remote_connect_back=0
      xdebug.remote_host=192.168.0.12

      ; idekey value is specific to Sublime
      xdebug.idekey=sublime.xdebug

      ; Optional: Set to true to always auto-start xdebug
      xdebug.remote_autostart=false

.. important:: Ensure you have retrieved the correct ``DockerNAT`` IP address as stated in the prerequisites section above!

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
