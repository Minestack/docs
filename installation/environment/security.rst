Security
========

Minestack takes a unique approach toward security by having dynamic usernames, passwords, and tokens for all supporting
services using Vault. To authenticate with Vault Minestack currently only supports the userpass backend however other
authentication backends will be supported in the future.

All Minestack services also authenticate against the Identity service. The only current authentication method for the
Identity service is username and password.

To ease the installation process this guide will not cover enabling SSL on service enpoints and supporting services.
For any supporting services only password/token security will be used. You can create secure passwords manually, generate
them using a tool such as `pwgen <https://sourceforge.net/projects/pwgen/>`_ or by running the following command:

.. code-block:: console

   $ openssl rand -hex 10

The bellow table includes all manually created passwords or tokens present in this guide.
Any auto generated passwords or tokens will not be listed.

+-------------------------+---------------------------------------------+
| Password Name           | Description                                 |
+=========================+=============================================+
| CONSUL_MASTER_TOKEN     | Master token for Consul                     |
+-------------------------+---------------------------------------------+
| VAULT_DB_PASSWORD       | Database password for Vault                 |
+-------------------------+---------------------------------------------+
| VAULT_RABBIT_PASSWORD   | RabbitMQ password for Vault                 |
+-------------------------+---------------------------------------------+
| RABBIT_ADMIN_PASSWORD   | RabbitMQ admin password                     |
+-------------------------+---------------------------------------------+
| ADMIN_TOKEN             | Admin token for the Identity service        |
+-------------------------+---------------------------------------------+
| ADMIN_PASSWORD          | Password of user :code:`admin`              |
+-------------------------+---------------------------------------------+
| IDENTITY_VAULT_PASSWORD | Vault password for the Identity service     |
+-------------------------+---------------------------------------------+
| REDSTONE_PASSWORD       | Password of the Compute service user        |
+-------------------------+---------------------------------------------+
| REDSTONE_VAULT_PASSWORD | Vault password for the Compute service      |
+-------------------------+---------------------------------------------+
| DASH_VAULT_PASSWORD     | Vault password for the Dashboard service    |
+-------------------------+---------------------------------------------+
| NFS_USER_PASSWORD       | NFS user password                           |
+-------------------------+---------------------------------------------+