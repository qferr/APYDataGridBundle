How to create a custom column type
==================================

The bundle comes with a bunch of core column types available for building grids.
However there are situations where you may want to create a custom column type for a specific purpose.

We suppose you want to create a column to show the video of the preview in a column.

Defining the column type
------------------------

First of all you have to extends the abstract :code:`Column` class and give a name to your new column.

.. code-block:: php

    <?php
    namespace AppBundle\Grid\Column;

    use APY\DataGridBundle\Grid\Column;

    class VideoColumn extends Column
    {
        public function getType()
        {
            return 'video';
        }
    }

The :code:`getName()` method returns an identifier which should be unique in your application.

Creating a template for the column
----------------------------------

.. code-block:: twig

    {# AppBundle:Grid:my_grid_template.html.twig #}
    {% extends 'APYDataGridBundle::blocks.html.twig' %}

    {% block grid_column_type_video_cell %}
        {# Show your player with the file path store in the variable {{ value }} #}
    {% endblock grid_column_type_video_cell %}

Using the column type
---------------------

You can now use your custom field type immediately, simply by creating a new instance of the type in one of your grids.

.. code-block:: php

    namespace AppBundle\Grid\Type;

    use AppBundle\Entity\Product;
    use AppBundle\Grid\Column\VideoColumn;
    use APY\DataGridBundle\Grid\GridBuilder;
    use APY\DataGridBundle\Grid\Type\GridType;

    class MovieType extends GridType
    {
        public function buildGrid(GridBuilder $builder, array $options)
        {
            $builder->add('video', new VideoColumn(), ['primary' => 'true']);
        }
    }

Creating the column type as a service
-------------------------------------

In a config file, register your column type as a service with the tag :code:`apy_grid.type`.

.. code-block:: yaml

    # src/AppBundle/Resources/config/forms.yml
    services:
        app.grid.column.type.video:
            class: AppBundle\Grid\Column\VideoColumn
            tags:
                - { name: apy_grid.type }

Use the column type is is now much easier:

.. code-block:: php

    $builder->add('video', new VideoColumn(), ['primary' => 'true']);
