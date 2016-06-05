Other Nodes
===========

Perform these steps on all other nodes.

Prerequisites
-------------

    .. todo::

       Insert Consul into the Minestack YUM repo

Install and Configure components
--------------------------------

    1. Install Consul:

       .. code-block:: console

          # yum install consul

    2. Edit the :code:`/etc/consul.conf` file:

        .. code-block:: conf

           {
           	"data_dir": "/var/lib/consul/",
           	"bind_addr": "10.0.0.31"
           	"datacenter": "dc1",
           	"retry_join": [
           	    "controller"
           	],
           	"ports": {
           		"dns": -1
           	}
           }

       .. note::

          Configuration may vary depending on the environment.


Finalize Installation
---------------------

    1. Start the Consul service and configure it to start when the system boots:

       .. code-block:: console

          # service enable consul
          # service start consul

Verify Operation
----------------

    1. Curl the status endpoint:

        .. code-block:: console

           $ curl -s http://127.0.0.1:8500/v1/status/peers

        .. note::

           It may take up to 30 seconds after starting for the consul agent to connect to the cluster.
