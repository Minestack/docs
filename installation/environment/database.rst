SQL Database
============

Most Minestack services use an SQL database to store information. The database typically runs on the controller node.
This guide uses PostgreSQL 9.5 but Minestack also supports other SQL databases including MySQL.

Prerequisites
-------------

1. Enable the PostgreSQL 9.5 package repository:

   .. code-block:: console

      # rpm -Uvh http://yum.postgresql.org/9.5/redhat/rhel-7-x86_64/pgdg-centos95-9.5-2.noarch.rpm

Install and Configure components
--------------------------------

1. Install the PostgreSQL 9.5 Server:

   .. code-block:: console

      # yum install postgresql95-server postgresql95

2. Initialize the server:

   .. code-block:: console

      # /usr/pgsql-9.5/bin/postgresql95-setup

   This command will take some time and will create a PostgreSQL data directory in :code:`/var/lib/pgsql/9.5/data/`

Finalize installation
---------------------

1. Start the PostgreSQL service and configure it to start when the system boots:

   .. code-block:: console

      # service enable postgresql-9.5
      # service start postgresql-9.5