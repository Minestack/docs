Compute Nodes
=============

Perform these steps on all compute nodes.

Install and Configure components
--------------------------------

    1. Install the packages:

       .. code-block:: console

          # yum install nfs-utils

    2. Create the share mount directory:

       .. code-block:: console

          # mkdir -p /mnt/nfs/minestack

    3. Edit the :code:`/etc/fstab` file to mount the share:

       .. code-block:: config

          controller:/var/nfsshare/minestack /mnt/nfs/minestack nfs defaults 0 0

    4. Reload the system mounts:

       .. code-block:: console

          # mount -a

Verify Operation
----------------

1. Read the test file

   .. code-block:: console

      $ cat /mnt/nfs/minestack/test

      Hello, World!