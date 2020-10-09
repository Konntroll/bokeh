.. _userguide_layout:

Creating layouts
================

Bokeh includes several layout options for plots and widgets. With these you 
can arrange multiple components to create interactive dashboards and data 
applications.

Layout functions let you build a grid of plots and widgets. You can have as
many rows, columns, or grids of plots in one layout as you'd like. Bokeh
layouts also allow for a number of sizing options, or modes. These modes 
allow plots and widgets to resize to fit the browser window.

.. _userguide_layout_layouts:

Basic layouts
-------------

Column layout
~~~~~~~~~~~~~

To display plots or widgets vertically, use the |column| function.

.. bokeh-plot:: docs/user_guide/examples/layout_vertical.py
    :source-position: above

Row layout
~~~~~~~~~~

To display plots horizontally, use the |row| function.

.. bokeh-plot:: docs/user_guide/examples/layout_horizontal.py
    :source-position: above

Grid layout for plots
~~~~~~~~~~~~~~~~~~~~~~

The |gridplot| function can be used to arrange Bokeh plots in a grid.
This funciton also merges all tools into a single toolbar. Each plot 
in the grid then has the same active tool.

You can leave grid cells blank by passing ``None`` to them instead of
a plot object.

.. bokeh-plot:: docs/user_guide/examples/layout_grid.py
    :source-position: above

For convenience, you can also just pass a list of plots and specify the
number of columns you want in your grid. For example:

.. code-block:: python

    gridplot([s1, s2, s3], ncols=2)

You can also pass in ``plot_width`` and ``plot_height`` arguments.
These dimensions will then appply to all your plots.

By default, |gridplot| merges all child plot tools into a single 
parent grid toolbar. To disable this behavior, set ``merge_tools``
to ``False``.

.. bokeh-plot:: docs/user_guide/examples/layout_grid_convenient.py
    :source-position: above

General grid layout
~~~~~~~~~~~~~~~~~~~

You can use the |layout| function to arrange plots and widgets into a grid.
This generates the necessary |row| and |column| layouts automatically and
lets you quickly create layouts. Try the following code as an example.

.. code-block:: python

    sliders = column(amp, freq, phase, offset)

    layout([
        [bollinger],
        [sliders, plot],
        [p1, p2, p3],
    ])


This produces the following layout:

.. image:: /_images/dashboard.png
    :width: 500px
    :height: 397px

|

The full code for this plot is available at
:bokeh-tree:`examples/howto/layouts/dashboard.py` in the project's GitHub
repository.

.. _userguide_layout_sizing_mode:

Sizing modes
------------

Modes
~~~~~

Layout-able Bokeh objects may be configured individually with the following
sizing modes:

``"fixed"``
    Component is not responsive. It will retain its original width and height
    regardless of any subsequent browser window resize events.

``"stretch_width"``
    Component will responsively resize to stretch to the available width, without
    maintaining any aspect ratio. The height of the component depends on the type
    of the component and may be fixed or fit to component's contents.

``"stretch_height"``
    Component will responsively resize to stretch to the available height, without
    maintaining any aspect ratio. The width of the component depends on the type
    of the component and may be fixed or fit to component's contents.

``"stretch_both"``
    Component is completely responsive, independently in width and height, and
    will occupy all the available horizontal and vertical space, even if this
    changes the aspect ratio of the component.

``"scale_width"``
    Component will responsively resize to stretch to the available width, while
    maintaining the original or provided aspect ratio.

``"scale_height"``
    Component will responsively resize to stretch to the available height, while
    maintaining the original or provided aspect ratio.

``"scale_both"``
    Component will responsively resize to both the available width and height,
    while maintaining the original or provided aspect ratio.

In general, either or both of ``width`` and ``height`` may also need to be
provided, depending on the mode. (e.g. for a ``stretch_width`` mode, the desired
fixed ``height`` must be provided).

Note that layout objects such as rows and columns will pass on their configured
sizing mode to any of their children that do not themselves have an explicitly
set ``sizing_mode`` of their own.

Single Object
~~~~~~~~~~~~~

The example below allows you to select a sizing mode from a dropdown to see
how a single plot responds to different modes:

.. bokeh-plot:: docs/user_guide/examples/layout_sizing_mode.py
    :source-position: none

.. note::
    If the enclosing DOM element does not define any specific height to fill,
    sizing modes that scale or stretch to height may shrink to a minimum size.

Multiple Objects
~~~~~~~~~~~~~~~~

Below is a more sophisticated (but fairly typical) example of a nested layout
with different sizing modes:

.. bokeh-plot:: docs/user_guide/examples/layout_sizing_mode_multiple.py
    :source-position: none

In the example above, the layout nests different subcomponents with various
different sizing modes:

.. code-block:: python

    # plot scales original aspect based on available width
    plot = figure(..., sizing_mode="scale_width")

    # sliders fill the space they are in
    amp = Slider(..., sizing_mode="stretch_both")

    # fixed sized for the entire column of sliders
    widgets = column(..., sizing_mode="fixed", height=250, width=150)

    # heading fills available width
    heading = Div(..., height=80, sizing_mode="stretch_width")

    # entire layout can fill the space it is in
    layout = column(heading, row(widgets, plot), sizing_mode="stretch_both")

.. _userguide_layout_limits:

Limitations
-----------

The Bokeh layout system is not a completely generic, general-purpose layout
engine. It intentionally sacrifices some capability in order to make common
use cases and scenarios simple to express. Extremely nested layouts with
many different sizing modes may yield undesirable results, either in terms of
performance, or visual appearance. For such cases, it is recommended to use the
methods in :ref:`userguide_embed` along with your own custom HTML templates in
order to take advantage of more sophisticated CSS layout possibilities.

.. |column|    replace:: :func:`~bokeh.layouts.column`
.. |gridplot|  replace:: :func:`~bokeh.layouts.gridplot`
.. |layout|    replace:: :func:`~bokeh.layouts.layout`
.. |row|       replace:: :func:`~bokeh.layouts.row`
