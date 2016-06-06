Vault
=====

Perform these steps on the controller node.

Prerequisites
-------------

    .. todo::

       Insert Vault into the Minestack YUM repo

    1. Generate a Consul token for Vault:

       a. Create the token json payload file :code:`/tmp/vaultct.json`

          .. code-block:: json

             {
               "Name":"vault",
               "Type": "management",
               "Rules": {
                 "key": {
                   "vault/": {
                     "policy": "write"
                   }
                 }
               }
             }

       b. Request the token

          .. code-block:: console

             # curl -s -X PUT http://controller:8500/v1/acl/create?token=CONSUL_MASTER_TOKEN \
                -d @/tmp/vaultct.json

             {
               "ID": "adf4238a-882b-9ddc-4a9d-5b6758e4159e"
             }

          Replace CONSUL_MASTER_TOKEN with the Consul master token.
          Save the returned token as you will be needing it later.

    2. Create a PostgreSQL user for Vault:

       a. Open the PostgreSQL console:

          .. code-block:: console

             # sudo -i -u postgres psql

       b. Create the Vault user:

          .. code-block:: sql

             CREATE ROLE vault WITH LOGIN CREATEROLE PASSWORD 'VAULT_DB_PASSWORD';

          Replace VAULT_DB_PASSWORD with a suitable password.

       .. note::

          You can exit the PostgreSQL console by using the :code:`\q` command.


Install and Configure components
--------------------------------

    1. Install Vault:

       .. code-block:: console

          # yum install vault

    2. Edit the :code:`/etc/vault.hcl` file:

       .. code-block:: conf

          backend "consul" {
            address = "controller:8500"
            path = "vault"
            token = "VAULT_CONSUL_TOKEN"
          }
          listener "tcp" {
            address = "10.0.0.11:8200"
            tls_disable = 1
          }

       Replace VAULT_CONSUL_TOKEN with the token we generated earlier.

       .. note::

          Configuration may vary depending on the environment.

    3. Start the Vault service and configure it to start when the system boots:

       .. code-block:: console

          # service enable vault
          # service start vault

    4. Initialize Vault:

       .. code-block:: console

          $ vault init

          Key 1: 427cd2c310be3b84fe69372e683a790e01
          Key 2: 0e2b8f3555b42a232f7ace6fe0e68eaf02
          Key 3: 37837e5559b322d0585a6e411614695403
          Key 4: 8dd72fd7d1af254de5f82d1270fd87ab04
          Key 5: b47fdeb7dda82dbe92d88d3c860f605005
          Initial Root Token: eaf5cc32-b48f-7785-5c94-90b5ce300e9b

          Vault initialized with 5 keys and a key threshold of 3!

          Please securely distribute the above keys. Whenever a Vault server
          is started, it must be unsealed with 3 (the threshold)
          of the keys above (any of the keys, as long as the total number equals
          the threshold).

          Vault does not store the original master key. If you lose the keys
          above such that you no longer have the minimum number (the
          threshold), then your Vault will not be able to be unsealed.

       .. warning::

          Please save all the keys and initial root token in a secure location. Anyone with access to these keys
          will have full access to everything in Vault.

    5. Unseal Vault:

       .. code-block:: console

          $ vault unseal

          Key (will be hidden):
          Sealed: true
          Key Shares: 5
          Key Threshold: 3
          Unseal Progress: 1

       .. note::

          You will need to enter 3 keys one at a time to unseal Vault. Vault will not know if the keys are valid
          until the threshold is reached.

       Continue entering keys until the Sealed status is false.

    6. Configure the Username & Password Auth Backend

        .. code-block:: console

           $ vault auth-enable userpass

           Successfully enabled 'userpass' at 'userpass'!

    7. Verify Operation

       .. code-block:: console

          $ curl -s http://controller:8200/v1/sys/health

          {
            "initialized": true,
            "sealed": false,
            "standby": false
          }


Configure the Consul Secret Backend
-----------------------------------

    1. Mount the Consul secret backend:

       .. code-block:: console

          $ vault mount consul

          Successfully mounted 'consul' at 'consul'!

    2. Configure the backend:

       .. code-block:: console

          $ vault write consul/config/access \
             address=controller:8500 \
             token=VAULT_CONSUL_TOKEN

          Success! Data written to: consul/config/access

       Replace VAULT_CONSUL_TOKEN with the token we generated earlier.

    3. Create the Minestack service Consul Role

       .. code-block:: console

          $ POLICY='service "" { policy = "read" } service "minestack-" { policy = "write" } check "service:minestack" { policy = "write" }'
          $ echo $POLICY | base64 | vault write consul/roles/minestack-service lease=1h policy=-

          Success! Data written to: consul/roles/minestack-service

    4. Verify Operation

       .. code-block:: console

          $ vault read consul/creds/minestack-service

          Key             Value
          lease_id        consul/creds/minestack-service/c7a3bd77-e9af-cfc4-9cba-377f0ef10e6c
          lease_duration  3600
          token           973a31ea-1ec4-c2de-0f63-623f477c2510

Configure the PostgreSQL Secret Backend
---------------------------------------

    1. Mount the PostgreSQL secret backend:

       .. code-block:: console

          $ vault mount postgresql

          Successfully mounted 'postgresql' at 'postgresql'!

    2. Configure the backend:

       .. code-block:: console

          $ vault write postgresql/config/connection \
             connection_url="postgresql://vault:VAULT_DB_PASSWORD@controller:5432/postgres?sslmode=disabled"

          Success! Data written to: postgresql/config/connection

       Replace VAULT_DB_PASSWORD with vault PostgreSQL user password.

       .. warning::

          We have not setup SSL for PostgreSQL connections. In a production environment SSL should be setup and enabled.

    3. Set the credential lease time:

       .. code-block:: console

          $ vault write postgresql/config/lease lease=1h lease_max=24

          Success! Data written to: postgresql/config/lease

Configure the RabbitMQ Secret Backend
---------------------------------------

    .. todo::

       Waiting for RabbitMQ Vault support