:orphan:

.. include:: /_includes/all.rst
.. include:: /_includes/snippets/__ANNOUNCEMENTS__.rst

.. _configure_php_xdebug_lin_phpstorm:

************************************
Docker on Linux: Xdebug for PhpStorm
************************************

Docker on Linux allows Xdebug to automatically connect back to the host system without the need
of an explicit IP address.

**Table of Contents**

.. contents:: :local:


Prerequisites
=============

Ensure you know how to customize ``php.ini`` values for the DreamDevBox and have a rough understanding
about common Xdebug options.

.. seealso::
   * :ref:`php_ini`
   * :ref:`configure_php_xdebug_options`


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

The **Resulting local project path** is the path where all projects are stored locally on your
host operating system. No matter what this path is, the equivalent remote path (inside the Docker
container) is always ``/shared/httpd``.

.. important:: Remember this, when it comes to path mapping in your IDE/editor configuration.


Configuration
=============

Configure PhpStorm
------------------

**1. Ensure Xdebug port is set to 9000**

   .. include:: /_includes/figures/xdebug/phpstorm-settings.rst

**2. Set path mapping**

   Create a new PHP server and set a path mapping. This tutorial assumes your local DreamDevBox projects
   to be in ``./data/www`` of the DreamDevBox git directory:

   .. include:: /_includes/figures/xdebug/phpstorm-path-mapping.rst

   .. important::
      Recall the path settings from the *Assumption* section and adjust if your configuration differs!

**3. Ensure DBGp proxy settings are configured**

   .. include:: /_includes/figures/xdebug/phpstorm-dbgp-proxy.rst

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
      :emphasize-lines: 7,10

      ; Defaults
      xdebug.default_enable=1
      xdebug.remote_enable=1
      xdebug.remote_port=9000

      ; The Linux way
      xdebug.remote_connect_back=1

      ; idekey value is specific to PhpStorm
      xdebug.idekey=PHPSTORM

      ; Optional: Set to true to always auto-start xdebug
      xdebug.remote_autostart=false

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
