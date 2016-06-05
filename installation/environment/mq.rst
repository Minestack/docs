Message Queue
=============

Minestack uses a message queue to coordinate operations and status information among services. The message queue typically
runs on the controller node. Minestack currently only supports RabbitMQ however other message queue systems may be supported
in the future.

Prerequisites
-------------

1. Install Erlang:

   .. code-block:: console

      # yum install https://www.rabbitmq.com/releases/erlang/erlang-18.3-1.el7.centos.x86_64.rpm

   .. note::

      This guide installs Erlang supplied by RabbitMQ however installing Erlang from EPEL or Erlang Solutions would work
      as well.

.. todo::

   Insert RabbitMQ and Erlang into the Minestack YUM repo

Install and Configure components
--------------------------------

    1. Install RabbitMQ:

       .. code-block:: console

          # yum install https://www.rabbitmq.com/releases/rabbitmq-server/v3.6.2/rabbitmq-server-3.6.2-1.noarch.rpm

       .. note::

          The RabbitMQ version used is the latest as of writing this guide. Newer versions of RabbitMQ 3 should work fine.

    2. Start the message queue service and configure it to start when the system boots:

       .. code-block:: console

          # systemctl enable rabbitmq-server
          # systemctl start rabbitmq-server

    3. Create the :code:`minestack` virtual host:

       .. code-block:: console

          # rabbitmqctl add_vhost minestack

    4. Remove the :code:`guest` user:

       .. code-block:: console

          # rabbitmqctl delete_user guest

    5. Add the :code:`admin` user:

       .. code-block:: console

          # rabbitmqctl add_user admin RABBIT_ADMIN_PASSWORD
          # rabbitmqctl set_user_tags admin administrator
          # rabbitmqctl set_permissions -p minestack admin ".*" ".*" ".*"

       Replace RABBIT_ADMIN_PASSWORD with a suitable password.

    6. Make the admin user an administrator and give them permissions on the :code:`minestack` vhost:

       .. code-block:: console

          # rabbitmqctl set_user_tags admin administrator
          # rabbitmqctl set_permissions -p minestack admin ".*" ".*" ".*"

    7. Enable the management plugin

       .. code-block:: console

          # rabbitmq-plugins enable rabbitmq_management

Verify Operation
----------------

    1. Go to the web UI located at: :code:`http://controller:15672/` and login with the admin user.
