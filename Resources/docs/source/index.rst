Welcome to APYDataGridBundle's documentation!
=============================================

.. toctree::
   :maxdepth: 2

Installation
------------

Download APYDataGridBundle using Composer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ composer require apy/datagrid-bundle

Composer will install the bundle to your project's ``vendor/apy/datagrid-bundle`` directory.

Enable the bundle
^^^^^^^^^^^^^^^^^

Enable the bundle in the kernel:

::

    <?php
    // app/AppKernel.php

    public function registerBundles()
    {
        $bundles = array(
            // ...
            new APY\DataGridBundle\APYDataGridBundle(),
        );
    }


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
