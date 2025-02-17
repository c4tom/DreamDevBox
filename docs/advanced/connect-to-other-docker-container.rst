.. include:: /_includes/all.rst
.. include:: /_includes/snippets/__ANNOUNCEMENTS__.rst

.. _connect_to_other_docker_container:

*********************************
Connect to other Docker container
*********************************

Other Docker container can either be accessed by connecting back to the host os or by adding its
image directly to the DreamDevBox stack.


**Table of Contents**

.. contents:: :local:


Any Docker container on host os
===============================

1. To connect to any other Docker container on your host os from within the DreamDevBox Docker
   container, you first need to make sure, you are able to connect to your host os from within the
   DreamDevBox Docker container.

   .. seealso:: :ref:`connect_to_host_os`

2. Once you are able to connect to the host os, start any other Docker container and make its
   port that you want to access available to your host os by specifying ``-p``.
   An example with e.g. an external |ext_lnk_docker_image_grafana| container
   might look like this:

   .. code-block:: bash

      host> docker run -d --name=grafana -p 3000:3000 grafana/grafana

   You can then connect to your host os on port ``3000`` from within the DreamDevBox Docker container
   and be able to use it.


Add Docker container to DreamDevBox network
========================================

The DreamDevBox defines its own bridge network, usually called ``dreamdevbox_app_net``.

.. note::
   The name may vary depending on the name of the DreamDevBox directory. It assembles itself by
   ``<DreamDevBox_dir_name>_app_net``.

1. Start the DreamDevBox
2. Start your container of choice

   .. code-block:: bash

      host> docker run -d --name mycontainer

3. Attach your container to the DreamDevBox network

   .. code-block:: bash

      host> docker network connect dreamdevbox_app_net mycontainer

Once you have done that, ``mycontainer`` is then part of the internal DreamDevBox network
and is able to resolve DreamDevBox container by its name and vice-versa.

4. Connect from DreamDevBox PHP container to ``mycontainer``

   From inside the PHP container, you can then refer to your container by its hostname
   ``mycontainer``


Add Docker container to DreamDevBox stack
======================================

Alternatively you can also add any Docker container to the DreamDevBox network by adding an image
it to the DreamDevBox stack directly.

.. seealso:: :ref:`add_your_own_docker_image`
