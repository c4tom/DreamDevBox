:orphan:

.. include:: /_includes/all.rst
.. include:: /_includes/snippets/__ANNOUNCEMENTS__.rst

.. _howto_add_custom_dns_server_on_linux:

******************************
Add custom DNS server on Linux
******************************

On Linux the DNS settings can be controlled by various different methods. Two of them are via
Network Manager and systemd-resolved. Choose on of the methods depending on your local setup.


**Table of Contents**

.. contents:: :local:


Assumption
==========

This tutorial is using ``127.0.0.1`` as the DNS server IP address, as it is the method to setup
Auto DNS for your local DreamDevBox.


Non permanent solution
=======================

When you just want to try out to add a new DNS server without permanent settings, you should use
this option.

.. note::
   Non permanent means, the settings will be gone when your DHCP release will be renewed,
   reconnecting to the network, restarting the network service, logging out or
   rebooting your machine.

1. Open ``/etc/resolv.conf`` with root or sudo privileges with your favourite editor on your
   host operating sustem:

   .. code-block:: bash

      host> sudo vi /etc/resolv.conf

2. Add your new ``nameserver`` directive **above** all existing nameserver directives:

   .. code-block:: bash
      :caption: /etc/resolv.conf
      :emphasize-lines: 3

      # Generated by NetworkManager
      search intranet
      nameserver 127.0.0.1
      nameserver 192.168.0.10

3. It will work instantly after saving the file


Network Manager
===============

*(This is a permanent solution and needs to be reverted when you don't need it anymore)*

Edit ``/etc/dhcp/dhclient.conf`` with root or sudo privileges and add an instruction, which tells
your local DHCP client that whenever any of your DNS servers are changed, you always want to have
an additional entry, which is the one from the DreamDevBox (``127.0.0.1``).

Add the following line to to the very beginning to ``/etc/dhcp/dhclient.conf``:

.. code-block:: bash
   :caption: /etc/dhcp/dhclient.conf

   prepend domain-name-servers 127.0.0.1;

When you do that for the first time, you need to restart the ``network-manager`` service.

.. code-block:: bash

   # Via service command
   host> sudo service network-manager restart

   # Or the systemd way
   host> sudo systemctl restart network-manager

This will make sure that whenever your /etc/resolv.conf is deployed, you will have ``127.0.0.1``
as the first entry and also make use of any other DNS server which are deployed via the LAN's DHCP
server.

If the DreamDevBox DNS server is not running, it does not affect the name resolution, because you will
still have other entries in ``/etc/resolv.conf``.


systemd-resolved
================

*(This is a permanent solution and needs to be reverted when you don't need it anymore)*

In case you are using systemd-resolved instead of NetworkManager, add the following line to
the very beginning to ``/etc/resolv.conf.head``:

.. code-block:: bash
   :caption: /etc/resolv.conf.head

   nameserver 127.0.0.1

Prevent NetworkManager from modifying ``/etc/resolv.conf`` and leave everything to
systemd-resolved by adding the following line under the ``[main]`` section of
``/etc/NetworkManager/NetworkManager.conf``

.. code-block:: bash
   :caption: /etc/NetworkManager/NetworkManager.conf

   dns=none

As a last step you will have to restart ``systemd-resolved``.

.. code-block:: bash

   host> sudo systemctl stop systemd-resolved
   host> sudo systemctl start systemd-resolved

Once done, you can verify if the new DNS settings are effective:

.. code-block:: bash

   host> systemd-resolve --status

.. seealso:: |ext_lnk_dns_archlinux_wiki_resolv_conf|
