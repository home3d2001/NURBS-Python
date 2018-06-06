Surface Generator
^^^^^^^^^^^^^^^^^

NURBS-Python comes with a simple surface generator which is designed to generate a control points grid to be used as
a randomized input to :py:class:`.BSpline.Surface` and :py:class:`.NURBS.Surface`. It is capable of generating
custom-sized surfaces with arbitrary divisions and generating hills (or bumps) on the surface. It is also possible to
export the surface as a text file in the format described under :doc:`File Formats <file_formats>` documentation.

The classes :py:class:`.CPGen.Grid` and :py:class:`.CPGen.GridWeighted` are responsible for generating surfaces and
they are documented under :doc:`Core Libraries <modules>`.

The following example illustrates a sample usage of the B-Spline surface generator:

.. code-block:: python

    # Example script: grid/ex_surfgen.py
    from geomdl import BSpline
    from geomdl import CPGen
    from geomdl import utilities
    from geomdl.visualization import VisPlotly

    # Generate a control points grid
    surfgrid = CPGen.Grid(50, 100)

    # Split the width into 5 equal pieces and the height into 10 equal pieces
    surfgrid.generate(5, 10)

    # Generate 4 bumps on the grid
    surfgrid.bumps(num_bumps=6, all_positive=False, bump_height=45)

    # Create a BSpline surface instance
    surf = BSpline.Surface()

    # Set degrees
    surf.degree_u = 3
    surf.degree_v = 3

    # Get the control points from the generated grid
    surf.ctrlpts2d = surfgrid.grid()

    # Set knot vectors
    surf.knotvector_u = utilities.generate_knot_vector(surf.degree_u, surf.ctrlpts_size_u)
    surf.knotvector_v = utilities.generate_knot_vector(surf.degree_v, surf.ctrlpts_size_v)

    # Split the surface at v = 0.35
    split_surf = surf.split_v(0.35)

    # Set sample size of the split surface
    split_surf.sample_size = 25

    # Generate the visualization component and its configuration
    vis_config = VisPlotly.VisConfig(ctrlpts=False, legend=False)
    vis_comp = VisPlotly.VisSurface(vis_config)

    # Set visualization component of the split surface
    split_surf.vis = vis_comp

    # Plot the split surface
    split_surf.render()
