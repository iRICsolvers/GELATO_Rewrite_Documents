Updates in GELATO Ver2.x
========================================================================================================================

The following points have been updated in GELATO Ver2.x compared to the previous versions of GELATO.
GELATO Ver2.x is a rewritten version that integrates the GELATO included in the iRIC installer and other derived versions.

Specification Changes
------------------------------------------------------------------------------------------------------------------------

Handling of Time Steps
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| The image below shows the handling of time steps in the previous versions of GELATO.
| The upper scale shows the time steps and times output from the flow calculation results, and the lower scale shows the time steps and times output by GELATO.

.. figure:: images/05/timestep_image_old.png
   :width: 50%
   :align: center

   : Handling of time steps in previous versions of GELATO

| As shown in the figure, GELATO could output results at finer time steps than the time steps of the loaded calculation results by using the Output Frequency Increase Factor, but the following problems existed:

- The initial time step of the loaded calculation results was not output.
- Changing the Output Frequency Increase Factor caused the time steps of the loaded calculation results and GELATO's time steps to not match.

| Due to the above specifications, the output intervals of tracers in GELATO were misaligned, and changing the Output Frequency Increase Factor resulted in different tracer tracking results.


| Therefore, in GELATO ver2.x, the initial state can now be output to solve the above problems, and the tracer tracking results do not change even if the Output Frequency Increase Factor is changed.
| The image below shows the handling of time steps in GELATO ver2.x.
| The pink triangles indicate which time steps of the flow calculation results are used for the substance transport tracking calculation in GELATO.

.. figure:: images/05/timestep_image_new.png
   :width: 50%
   :align: center

   : Handling of time steps in GELATO ver2.x

| Additionally, GELATO ver2.x has a feature to output using the original time of the loaded data.
| For example, if the initial time of the loaded calculation results is 200 seconds, you can choose whether the output time in GELATO starts from 0 seconds or 200 seconds.
| Note that if you use the original time, you need to set the tracer dispersion time and calculation end time in the original time as well.

.. figure:: images/05/timestep_output.png
   :width: 50%
   :align: center

   : Setting the time for output

Loading Flow Calculation Results
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| Previously, after starting the solver, the user had to import the grid and specify the path of the calculation results to be loaded in the calculation results dialog.
| In GELATO ver2.x, it is possible to specify the path of the calculation results directly from the dialog displayed when starting the solver. This makes it easier to import the grid and load the calculation results.
| Additionally, with this change, you can check information such as which solver was used to calculate the loaded results in the calculation results dialog.

.. figure:: images/05/cgns_file_new.png
   :width: 600pt
   :align: center

   : Information displayed in the calculation conditions dialog

| Furthermore, previously, an error occurred when reading flow velocity or water depth from the calculation results if the calculation result names differed from those set in GELATO.

.. note::
   For example, for water depth, it could only be read if the solver output the name as :guilabel:`Depth(m)`, :guilabel:`Depth[m]`, :guilabel:`Depth`, or :guilabel:`depth(m)`.

| Therefore, in GELATO ver2.x, the specification has been changed to allow the user to select the target by checking what names of calculation results are in the CGNS file of the loaded calculation results.
| As a result, although it no longer automatically reads as before, it is possible to read any CGNS file that outputs flow velocity, water depth, or bed elevation, regardless of which solver was used to calculate it.

.. figure:: images/05/select_result_new.png
   :width: 600pt
   :align: center

   : Selecting calculation results

.. _Setting Parameters for Fish Simulation:

Setting Parameters for Fish Simulation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| In GELATO ver2.x, the method for setting swimming parameters for fish has been significantly changed from previous versions.
| Previously, random body lengths within three patterns were set based on the representative body length of the fish, and variations based on body length were also applied to swimming speed, etc.
| However, this method had the problem that it was difficult to create multiple groups of fish with arbitrary parameters because the parameters such as body length and swimming speed were set randomly.

| Therefore, in GELATO ver2.x, arbitrary parameters can be set for each group of fish using the new features of iRIC. Accordingly, the fish setting dialog has also been significantly changed.

.. figure:: images/05/new_fish_setting_dialog_01.png
   :width: 500pt
   :align: center

   : Fish setting dialog in GELATO ver2.x

| Users create groups of fish in the iRIC calculation conditions dialog and set parameters such as body length, swimming speed, and swimming direction for each group. Two input modes are available: list display mode for each group and table format mode where parameters for each group can be checked at a glance.

.. figure:: images/05/new_fish_setting_dialog_02.png
   :width: 500pt
   :align: center

   : Fish setting dialog in GELATO ver2.x (list display mode)

.. figure:: images/05/new_fish_setting_dialog_03.png
   :width: 100%
   :align: center

   : Fish setting dialog in GELATO ver2.x (table format mode)

| Additionally, in the parameter input dialog using this new feature, parameters for each group of fish can be imported and exported in CSV format, allowing parameters created in Excel, etc., to be loaded in bulk.

.. figure:: images/05/fish_parameter_csv.png
   :width: 100%
   :align: center

   : Example of fish parameters in CSV format

| In conjunction with this change, a macro for Microsoft Excel has been created to easily create fish setting files (.csv). By using this macro, it is possible to set parameters randomly based on a standard body length as before, as well as set body lengths at equal intervals within a specified range.
| This macro can be downloaded from `here <https://i-ric.org/en/download/gelato_fishfilemaker/>`_.

.. figure:: images/05/fish_parameter_macro.png
   :width: 100%
   :align: center

   : Macro for fish parameters


Additional Features
------------------------------------------------------------------------------------------------------------------------

Setting Manning's Roughness Coefficient
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Previously, the Manning's roughness coefficient used in calculations within GELATO was a uniform value determined within the program, but now users can assign an arbitrary constant value or use values mapped to the calculation grid.

.. figure:: images/05/manning.png
   :width: 600pt
   :align: center

   : Setting Manning's roughness coefficient

Additional Feature for Total Number of Tracers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| In GELATO ver2.x, a feature has been added to output the number of tracers present within the calculation range at each time step.
| By using iRIC's graph and label features, it is easy to check the temporal changes in the total number of tracers.
| This feature applies not only to regular tracers but also to trajectory tracking tracers and fish.

.. figure:: images/05/tracer_number_graph.png
   :width: 600pt
   :align: center

   : Temporal changes in the total number of tracers

.. figure:: images/05/tracer_number_label.png
   :width: 600pt
   :align: center

   : Label display of the total number of tracers

Handling When Fish Enter Depths Below Activity Limit
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| In previous versions of GELATO, fish would disappear if they entered areas with depths below their activity limit due to changes in water level or movement.
| In ver2.x, if fish enter areas with depths below their activity limit, they are moved back to their previous location and one of the following options is executed.
| Note that if they enter areas with depths below their activity limit again after being moved, they will not be moved again.

- Stop at that location (stop at the pre-move location)
- Swim in the opposite direction
- Drift with the flow
- Swim in a random direction
- Be removed

Additional Information for Fish
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| Previously, the information output to the fish polygons was only the Type, which only identified groups of fish with the same body length, and if the jump feature was enabled, the type was replaced with information on whether the fish were jumping or not.
| In ver2.x, the following information is added to the fish polygons:

- Index of the group the fish belongs to
- Fish index
- Jump status (whether the fish jumped between time steps)
- Cruising status (whether the fish is cruising or dashing)
- Below activity limit mode (whether the fish is in the period of handling below activity limit at the time of output)

.. figure:: images/05/fish_info.png
   :width: 300pt
   :align: center

   : Information added to fish


Additional Feature for Tracer Capture
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| An attribute called :guilabel:`Tracer Trap` has been added to the cell attributes of the calculation grid.
| By assigning a capture rate to the cell, a feature has been added that captures tracers entering the cell at the set capture rate and stops their movement.

Drawing Tree and Gravel Polygons
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| Previously, only one tree or gravel polygon could be randomly placed within each specified drawing range, but in GELATO ver2.x, a feature has been added to place multiple tree or gravel polygons within the specified range.
| Additionally, tree polygons could only be drawn with the Y-axis direction up, but in GELATO ver2.x, users can specify the angle when drawing.

Generating Different Random Patterns
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| Previously, the random pattern generation was fixed, so the movement due to random walk, etc., was output in the same pattern no matter how many times the same calculation conditions were recalculated.
| Therefore, a feature has been added to change the seed value of the random number to perform calculations with different random patterns even with the same calculation conditions.

For details, see :ref:`Random Seed Value` 

Bug Fixes
------------------------------------------------------------------------------------------------------------------------

Adjusting the Range and Period for Adding Tracers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| Previously, due to decimal point errors, the boundary positions and times were not included in the target range when adding tracers, etc., causing misalignment.
| In GELATO ver2.x, the misalignment due to decimal point errors has been corrected, ensuring the exact number of tracers is added.

| For example, when adding tracers in the range :math:`10.0\leq t \leq20.0` with :math:`Δt=0.1`, previously tracers were not added at the boundaries :math:`t=10.0` and :math:`t=20.0`, but in GELATO ver2.x, the boundary values are stably included in the range.
| If the same value is set for the minimum and maximum values, only that value is targeted (e.g., if set to :math:`10.0\leq t \leq10.0`, tracers are added only once at :math:`t=10.0` regardless of the interval value).

| This makes it easier to set parameters when adding regular tracers at arbitrary times.

Fixing the Bug with the Cloning Reduction Factor for Empty Cells Cloning for Generating Tracers in All Empty Cells
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| Previously, when the :guilabel:`cloning reduction factor for empty cells cloning` was set to 2 or more, the remainder of the total number of cells divided by the :guilabel:`cloning reduction factor for empty cells cloning` affected the next tracer generation, causing the position where tracers were generated to shift each time.
| In GELATO ver2.x, this has been fixed so that the remainder of the total number of cells divided by the :guilabel:`cloning reduction factor for empty cells cloning` does not affect the next tracer generation.

Background Flow Conditions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| In GELATO, there is a feature to use the flow velocity entered in the GUI for tracer tracking, but a bug where the eddy viscosity coefficient was not calculated when using this feature has been fixed.

Particle Coupling Feature
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| It was confirmed that the coupling feature did not work properly in previous versions of GELATO, so the particle coupling feature is not included in GELATO ver2.0.

Displaying Windmap
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| The value of :guilabel:`Windmap lines` output as an attribute of Windmap was supposed to be a generalized flow velocity value in the range of 0 to 1, but a bug where the correct value was not output has been fixed.
| In GELATO ver2.x, two attributes are output: :guilabel:`Length`, which is the length of the windmap lines, and :guilabel:`Normalized Velocity`, which is the generalized velocity.

Drawing Tree and Gravel Polygons
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- A bug where tree and gravel polygons were drawn even if their positions were deeper than the specified depth has been fixed.
- A bug where tree and gravel polygons were drawn outside the grid range has been fixed.
