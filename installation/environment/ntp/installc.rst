Controller Node
===============

Perform these steps on the controller node.

Install and Configure components
--------------------------------

    1. Install the packages:

        .. code-block:: console

           # yum install chrony

    2. Edit the :code:`/etc/chrony.conf` file and add, change, or remove the following keys as necessary for
       your environment:

        .. code-block:: conf

           server NTP_SERVER iburst

       Replace CONSUL_MASTER_TOKEN with that random

       .. note:

          By default, the controller node synchronizes the time via a pool of public servers. However, you can optionally
          configure alternative servers.


Finalize Installation
---------------------

    1. Start the NTP service and configure it to start when the system boots:

        .. code-block:: console

           # systemctl enable chronyd
           # systemctl start chronyd

