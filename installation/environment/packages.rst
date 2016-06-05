Minestack Packages
==================

Perform these steps on all hosts.

.. note::

   Disable or remove any automatic update services because they can impact your Minestack environment.

Prerequisites
-------------

    1. Upgrade the packages on your host:

       .. code-block:: console

          # yum upgrade

       .. note::

          If the upgrade process includes a new kernel, reboot your host to activate it.

    2. Install Python pip:

        .. code-block:: console

           # yum install python-pip

Enable the Minestack repository
-------------------------------

    .. todo::

       Build python packages into RPMs and distribute on PyPi.
       Create a Minestack YUM repository

Finalize the installation
-------------------------

    1. Install the Minestack client:

        .. code-block:: console

           # pip install https://github.com/minestack/python-minestack-client/archive/someversion.zip
