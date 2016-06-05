Controller Node
===============

Perform these steps on the controller node.

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
           	"server": true,
           	"bind_addr": "10.0.0.11"
           	"bootstrap": true,
           	"ui": true,
           	"datacenter": "dc1",
           	"acl_datacenter": "dc1",
           	"acl_default_policy": "deny",
           	"acl_master_token": "CONSUL_MASTER_TOKEN",
           	"ports": {
           		"dns": -1
           	}
           }

       Replace CONSUL_MASTER_TOKEN with a suitable token.

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

           $ curl -s http://controller:8500/v1/status/peers
