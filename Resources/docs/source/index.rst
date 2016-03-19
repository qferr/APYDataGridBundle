Welcome to APYDataGridBundle's documentation!
=============================================

- Supports Entity (ORM), Document (ODM) and Vector (Array) sources
- Sortable and Filterable with operators (Comparison operators, range, starts/ends with, (not) contains, is (not) defined, regex)
- Auto-typing columns (Text, Number, Boolean, Array, DateTime, Date, ...)
- Locale support for DateTime, Date and Number columns (Decimal, Currency, Percent, Duration, Scientific, Spell out)
- Input, Select, checkbox and radio button filters filled with the data of the grid or an array of values
- Export (CSV, Excel, _PDF_, XML, JSON, HTML, ...)
- Mass actions
- Row actions
- Supports mapped fields with Entity source
- Securing the columns, actions and export with security roles
- Annotations and PHP configuration
- External filters box
- Ajax loading
- Pagination (You can also use Pagerfanta)
- Column width and column align
- Prefix translated titles
- Grid manager for multi-grid on the same page
- Groups configuration for ORM and ODM sources
- Easy templates overriding (twig)
- Custom columns and filters creation
- ...

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

.. code-block:: php

    <?php
    // app/AppKernel.php

    public function registerBundles()
    {
        $bundles = array(
            // ...
            new APY\DataGridBundle\APYDataGridBundle(),
        );
    }


Creating a simple Grid
----------------------

Suppose you want to display a product list.
But before you begin, first focus on the Product class that represents and stores the data for a single product.

.. code-block:: php

    namespace AppBundle\Entity;

    class Product
    {
        /**
         * @var int
         */
        protected $id;

        /**
         * @var string
         */
        protected $name;

        /**
         * @var DateTime
         */
        protected $createdAt;

        /**
         * @var string
         */
        protected $status;

        // Getters and setters ...
    }

Building the grid
^^^^^^^^^^^^^^^^^

Now that you've created a Product class, the next step is to create and render the product grid.
In Symfony, this is done by building a grid object and then rendering it in a template.
For now, this can all be done from inside a controller:

.. code-block:: php

    namespace AppBundle\Controller;

    use AppBundle\Entity\Product;
    use APY\DataGridBundle\Grid\Source\Entity;
    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Request;

    class ProductController extends Controller
    {
        public function listAction(Request $request)
        {
            // Creates the builder
            $grid = $this->createGridBuilder(new Entity('AppBundle:Product'));
                ->add('id', 'numeric', ['primary' => 'true'])
                ->add('name', 'text')
                ->add('created_at', 'datetime', ['field' => 'createdAt'])
                ->add('status', 'text')
                ->getGrid();

            // Handles filters, sort, exports, action
            $grid->handleRequest($request);

            // Renders the grid
            return $this->render('AppBundle:Product:list', ['grid' => $grid]);
        }

        /**
         * @return GridBuilder
         */
        public function createGridBuilder(Source $source = null, array $options = [])
        {
            return $this->container->get('apy_grid.factory')->createBuilder('grid', $source, $options);
        }
    }

Creating a grid requires relatively little code because the grid objects are built with a "grid builder".
The grid builder's purpose is to allow you to write simple grid, and have it do all the heavy-lifting of actually building the grid.

Rendering the grid
^^^^^^^^^^^^^^^^^^

Now that the grid has been created, the next step is to render it.
This is done by passing the grid object to your template and using a set of form helper functions:

.. code-block:: twig

    {# app/Resources/views/product/list.html.twig #}
    {{ grid(grid) }}

Handling filters, sort, exports and actions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: php

    namespace AppBundle\Controller;

    use AppBundle\Entity\Product;
    use APY\DataGridBundle\Grid\Source\Entity;
    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Request;

    class ProductController extends Controller
    {
        public function listAction(Request $request)
        {
            // Creates the builder
            $grid = $this->createGridBuilder(new Entity('AppBundle:Product'));
                ->add('id', 'numeric', ['primary' => 'true'])
                ->add('name', 'text')
                ->add('created_at', 'datetime', ['field' => 'createdAt'])
                ->add('status', 'text')
                ->getGrid();

            // Handles filters, sort, exports, action
            $grid->handleRequest($request);

            if ($grid->isReadyForExport()) {
                return $grid->getExportResponse();
            }

            if ($grid->isReadyForRedirect()) {
                return new RedirectResponse($grid->getRouteUrl());
            }

            // Renders the grid
            return $this->render('AppBundle:Product:list', ['grid' => $grid]);
        }

        /**
         * @return GridBuilder
         */
        public function createGridBuilder(Source $source = null, array $options = [])
        {
            return $this->container->get('apy_grid.factory')->createBuilder('grid', $source, $options);
        }
    }

Columns Types
-------------

The bundle comes with a large group of column types:

- :doc:`Text Column </columns/types/text>`
- Number Column
    - Decimal
    - Currency
    - Percent
    - Duration
    - Scientific
    - Spell Out
- Boolean Column
- DateTime Column
- Date Column
- Time Column
- Array Column
- Blank Column
- Rank Column
- Join Column

You can also create your own custom column types.
This topic is covered in the ":doc:`/cookbook/column/create_column_type`" article of the cookbook.
