.. include:: /_includes/all.rst
.. include:: /_includes/snippets/__ANNOUNCEMENTS__.rst

.. _example_setup_joomla:

************
Setup Joomla
************

This example will use ``wget`` to install Joomla from within the DreamDevBox PHP container.

After completing the below listed steps, you will have a working Joomla setup ready to be
served via http and https.

.. seealso:: |ext_lnk_example_joomla_documentation|


**Table of Contents**

.. contents:: :local:


Overview
========

The following configuration will be used:

+--------------+--------------------------+-------------+------------+-------------------------------------------------+
| Project name | VirtualHost directory    | Database    | TLD_SUFFIX | Project URL                                     |
+==============+==========================+=============+============+=================================================+
| my-joomla    | /shared/httpd/my-joomla  | n.a.        | loc        | http://my-joomla.loc |br| https://my-joomla.loc |
+--------------+--------------------------+-------------+------------+-------------------------------------------------+

.. note::
   * Inside the DreamDevBox PHP container, projects are always in ``/shared/httpd/``.
   * On your host operating system, projects are by default in ``./data/www/`` inside the
     DreamDevBox git directory. This path can be changed via :ref:`env_httpd_datadir`.


Walk through
============

It will be ready in six simple steps:

1. Enter the PHP container
2. Create a new VirtualHost directory
3. Download and extract Joomla
4. Symlink webroot directory
5. Setup DNS record
6. Visit http://my-joomla.loc in your browser


1. Enter the PHP container
--------------------------

All work will be done inside the PHP container as it provides you with all required command line
tools.

Navigate to the DreamDevBox git directory and execute ``shell.sh`` (or ``shell.bat`` on Windows) to
enter the running PHP container.

.. code-block:: bash

   host> ./shell.sh

.. seealso::
   * :ref:`enter_the_php_container`
   * :ref:`work_inside_the_php_container`
   * :ref:`available_tools`


2. Create new vhost directory
-----------------------------

The vhost directory defines the name under which your project will be available. |br|
( ``<vhost dir>.TLD_SUFFIX`` will be the final URL ).

.. code-block:: bash

   dreamdevbox@php-7.0.20 in /shared/httpd $ mkdir my-joomla

.. seealso:: :ref:`env_tld_suffix`


3. Download and extract Joomla
------------------------------

Navigate into your newly created vhost directory and install Joomla.

.. code-block:: bash

   dreamdevbox@php-7.0.20 in /shared/httpd $ cd my-joomla
   dreamdevbox@php-7.0.20 in /shared/httpd/my-joomla $ wget -O joomla.tar.gz https://downloads.joomla.org/cms/joomla3/3-8-0/joomla_3-8-0-stable-full_package-tar-gz?format=gz
   dreamdevbox@php-7.0.20 in /shared/httpd/my-joomla $ mkdir joomla
   dreamdevbox@php-7.0.20 in /shared/httpd/my-joomla $ tar xvfz joomla.tar.gz -C joomla/

How does the directory structure look after installation:

.. code-block:: bash

   dreamdevbox@php-7.0.20 in /shared/httpd/my-joomla $ tree -L 1
   .
   ├── joomla.tar.gz
   └── joomla

   1 directory, 1 file


4. Symlink webroot
------------------

Symlinking the actual webroot directory to ``htdocs`` is important. The web server expects every
project's document root to be in ``<vhost dir>/htdocs/``. This is the path where it will serve
the files. This is also the path where your frameworks entrypoint (usually ``index.php``) should
be found.

Some frameworks however provide its actual content in nested directories of unknown levels.
This would be impossible to figure out by the web server, so you manually have to symlink it back
to its expected path.

.. code-block:: bash

   dreamdevbox@php-7.0.20 in /shared/httpd/my-joomla $ ln -s joomla/ htdocs

How does the directory structure look after symlinking it:

.. code-block:: bash

   dreamdevbox@php-7.0.20 in /shared/httpd/my-joomla $ tree -L 1
   .
   ├── joomla.tar.gz
   ├── joomla
   └── htdocs -> joomla

   2 directories, 1 file

As you can see from the above directory structure, ``htdocs`` is available in its expected
path and points to the frameworks entrypoint.

.. important::
   When using **Docker Toolbox**, you need to **explicitly allow** the usage of **symlinks**.
   See below for instructions:

   * Docker Toolbox and :ref:`howto_docker_toolbox_and_the_dreamdevbox_windows_symlinks`


5. DNS record
-------------
If you **have** Auto DNS configured already, you can skip this section, because DNS entries will
be available automatically by the bundled DNS server.

If you **don't have** Auto DNS configured, you will need to add the following line to your
host operating systems ``/etc/hosts`` file (or ``C:\Windows\System32\drivers\etc`` on Windows):

.. code-block:: bash
   :caption: /etc/hosts

   127.0.0.1 my-joomla.loc

.. seealso::

   * :ref:`howto_add_project_hosts_entry_on_mac`
   * :ref:`howto_add_project_hosts_entry_on_win`
   * :ref:`setup_auto_dns`


6. Open your browser
--------------------

All set now, you can visit http://my-joomla.loc or https://my-joomla.loc in your browser.


Next steps
==========

.. include:: /_includes/snippets/examples/next-steps.rst
