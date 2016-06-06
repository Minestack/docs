Controller Node
===============

Perform these steps on the controller node.

Install and Configure components
--------------------------------

    1. Install the packages:

       .. code-block:: console

          # yum install nfs-utils

    2. Create the share directories:

       .. code-block:: console

          # mkdir -p /var/nfsshare/minestack
          # mkdir /var/nfsshare/minestack/server
          # mkdir /var/nfsshare/minestack/bungee
          # mkdir /var/nfsshare/minestack/plugins
          # mkdir /var/nfsshare/minestack/worlds

    3. Create the :code:`minestack` user for the share and set permissions:

       .. code-block:: console

          # useradd -d /var/nfsshare/minestack -M -s /bin/bash
          # chown -R minestack:minestack /var/nfsshare/minestack

       .. note::

          This user is used to upload Minecraft server files

    4. Set the password for the :code:`minestack` user:

       .. code-block:: console

          # passwd minestack

       Save this password as the NFS_USER_PASSWORD.

    5. Edit the :code:`/etc/exports` to add the NFS export:

       .. code-block:: config

          /var/nfsshare/minestack compute(ro,sync,all_squash)

    6. Write a test file to the share:

       .. code-block:: console

          # sudo -i -u minestack echo "Hello, World!" > /var/nfsshare/minestack/test

Finalize Installation
---------------------

    1. Start the NFS service and configure it to start when the system boots:

        .. code-block:: console

           # systemctl enable rpcbind
           # systemctl enable nfs-server
           # systemctl enable nfs-lock
           # systemctl enable nfs-idmap
           # systemctl start rpcbind
           # systemctl start nfs-server
           # systemctl start nfs-lock
           # systemctl start nfs-idmap