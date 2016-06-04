Controller Node
===============

Perform these steps on the controller node.

Install and Configure components
--------------------------------

    1. Install the packages:

        .. code-block:: console

           # yum install chrony

    2. Edit the :code:`/etc/chrony.conf` and add, change, or remove the following keys as necessary for
       your environment:

        .. code-block:: conf

           server NTP_SERVER iburst

       Replace NTP_SERVER with the hostname or IP address of a suitable more accurate NTP server. The configuration
       supports multiple server keys.

       .. note:

          By default, the controller node synchronizes the time via a pool of public servers. However, you can optionally
          configure alternative servers.

    3. To enable other nodes to connect to the chrony daemon on the controller, add the following key to the
    :code:`/etc/chrony.conf` file:

        .. code-block:: conf

           allow 10.0.0.0/24

       If necessary, replace :code:`10.0.0.0/24` with a description of your subnet.


Finalize Installation
---------------------

    1. Start the NTP service and configure it to start when the system boots:

        .. code-block:: console

           # systemctl enable chronyd
           # systemctl start chronyd

