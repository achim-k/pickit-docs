Picking
=======

Pick-it comes with a set of options that allows configuring how detected
objects should be picked by a robot. These options, that can be found in
the  **Picking** tab of the Pick-it web interface, are grouped in the
following categories:

.. contents::
   :depth: 1
   :local:

This article explains how to use all settings to get the most out of
your application. You will learn how to align object frames for picking,
prevent collisions with a bin or other objects and choose the pick
order.

Note on frames
--------------

Before we start, let's first define three frame types in Pick-it that
we'll need throughout the article.

-  **Reference frame: **\ The reference frame defines the location of
   your workspace in which to find objects. It usually has a Z axis
   (blue) that points up.
-  The \ **object frame** is the default frame that Pick-it associates
   to a shape. It indicates where a detected object is located and how
   it is oriented. It is chosen by the Pick-it detection algorithms and
   cannot be modified. This frame might not always be the best choice to
   use for picking, which is why the pick frame exists.
-  The \ **pick frame** is what gets sent to the robot. It defines how a
   robot can pick or grasp a detected object and is derived from the
   object frame by changing settings in the Picking tab of the web
   interface.

|image0|

Pick strategy
-------------

Object to reference frame alignment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This setting aligns an object frame axis with a reference frame axis.
The newly created aligned frame is the pick frame that will be sent to
the robot. The setting smartly makes use of the symmetry of an object to
decide if object axes can be flipped so they point to the same direction
as the selected reference frame axes.

.. note:: This setting is only available with the **Flex** and **Teach engine**.

Two options need to be provided to make this work:

-  The \ **axis of the object frame** to align. The provided options are
   X or Y.
-  And the \ **axis of the reference frame** to align the object axis
   with. The provided options are X, -X, Y, -Y, Z or -Z.

The pictures below show examples of the object to reference frame
alignment setting for rectangular and circular shapes respectively:

|image1|

Pick strategy
~~~~~~~~~~~~~

.. note:: This setting is only available for **cylinder** and **sphere** detections with the **Flex engine**.


This setting allows you to move the pick frame to the real surface of
the detected object instead of the center of the object. The possible
options are:

-  **Default: **\ The default object frame will be used, which is a
   frame in the center of the cylinder or sphere. This option does not
   modify the pick frame.
-  **Surface top: **\ The pick frame will move to the center at the top
   surface of the object.
-  **Surface visible: **\ The pick frame will move to the center of the
   visible part of the object. This setting is useful to avoid
   collisions when the detected object is partially covered by another
   object.
-  **End circle: **\ The pick frame will be moved to the highest
   circular end of the cylinder.

.. note:: The resulting pick frame axes will not necessarily be parallel
   to one of the reference frame axes. If you want this, use the **Enforce
   alignment of pick frame orientation** option.

The picture below shows an example of a spherical and cylindrical object
respectively:

|image2|

Enforce alignment of Pick frame orientation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This setting can be used to enforce aligning an object frame with the
reference frame. The newly created aligned frame is the pick frame that
will be sent to the robot. This setting will make sure that one or more
resulting pick frame axes have a parallel or perpendicular axis to the
reference frame axes.

.. attention:: Enforcing a pick frame orientation takes precedence over the object 
   to reference frame alignment.

There are multiple alignment options:

-  **No alignment:** No alignment will be done, this option does not
   modify the pick frame.
-  **Y ⊥ Z: **\ Aligns the Y-axis of the pick frame to be in the XY
   plane of the reference frame.
-  **Z \|\| Z:** Aligns the Z-axis of the pick frame to be parallel to
   the Z axis of the reference frame. In most applications, the Z axis
   points up from the table or bin, so this option enforces the pick
   frame to point upwards.
-  **XYZ \|\| XYZ:** Aligns all three axis of the pick frame with all
   three axis of the reference frame.

When any of the alignment options is selected (except for 'No
alignment'), the following additional options appear. It is recommended
to leave these options to their default values or contact a support
engineer to set them.

-  **Distance from box for avoidance:** Default value 30 mm.
-  **Angular modification away from box:** Default value 20 degrees.
-  **Allowed correction axis deviation:** Default value 0 degrees.
-  **Allowed correction along pick frame Y axis:** Default value 20
   degrees.
-  **Avoid ROI sides treating object as:** Default value Preserve type.

The picture below shows an example of a bin with cylinders:

|image3|

Collision prevention
--------------------

This section explains how to prevent collisions when picking objects
with a robot. Objects that will not be picked because of collision
constraints will be labeled as unpickable and not sent to the robot. In
the Pick-it web interface, unpickable objects are displayed orange in
the Objects view and the  `detection
grid <https://support.pickit3d.com/article/167-the-pick-it-detection-grid>`__.

Maximum angle between pick and reference frame Z-axis
-----------------------------------------------------

With this setting, you can specify the maximum angular difference
between the Z axis of your pick frame and the Z axis of your reference
frame. If an object is tilted more than the maximum specified angle, the
object will be labeled as unpickable and not sent to the robot. In the
Pick-it web interface, unpickable objects are displayed orange in the
Objects view and the  `detection
grid <https://support.pickit3d.com/article/167-the-pick-it-detection-grid>`__.

Diameter of cylindrical tool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Pick-it detects collisions by modeling the robot tool as a cylinder.
There are two parameters that you need to provide: 'Diameter of the
cylindrical tool' and 'Tool to pick frame Z-axis offset' (see next
setting). If your tool is not cylindrical you should define the widest
part of your tool as the diameter of the cylinder.

Tool to pick frame Z axis offset
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Additionally to the diameter of the cylindrical tool, you can specify a
Z-axis offset between the tool and pick frame. Usually, a robot tool has
a thin extremity and gets thicker towards the robot flange. To ignore
this thin extremity for collision checks, specify the length of the
length in the 'tool to pick frame Z axis offset'.

Check collisions with
~~~~~~~~~~~~~~~~~~~~~

This setting specifies for which obstacles collisions need to be
checked. The options are:

-  **Bin:** Prevents collisions between robot tool and region of
   interest. Typically the region of interest box boundaries corresponds
   to the boundaries of a bin in a bin-picking scenario.

   .. attention:: Bin avoidance constraints take precedence over other
      specified pick frame alignment constraints.

-  **Other objects:** Prevents collisions between the robot tool model
   and objects different from the one to pick.
-  **Robot motions:** Prevents collisions with the robot arm while it is
   moving towards an object. Imagine when a pile of things is standing
   in the way of the actual object that you want to pick, you don't want
   to make this pile collapse.

   -  **Position of collision-free volume:** Specifies on which side of
      the region of interest box, the robot is standing.
   -  **Length scaling factor (%) of collision-free
      volume: **\ Specifies the size of the volume around the object
      where the system doesn't check for other objects that are standing
      in the way.
   -  **Height of collision-free volume above reference
      frame:** Specifies the minimum height of the pile of objects for
      which the system needs to check collisions.

Object ordering
---------------

Under object ordering, you can define in which order objects will be
picked when more than one object is detected. Options are:

-  **Highest product center:** Sort objects with highest product center
   first. This is the most common option.
-  **Lowest product center:** Reverse ordering from 'Highest product
   center'.
-  **Highest product part:** Sort objects with highest volume or surface
   boundary first.
-  **Lowest X value first:** Orders objects based on object center
   position. From small to large X value.
-  **Highest X value first:** Reverse ordering from 'Lowest X value
   first'.
-  **Lowest Y value first:** Orders objects based on object center
   position. From small to large Y value.
-  **Highest Y value first:** Reverse ordering from 'Lowest Y value
   first'.
-  **Biggest product:** Objects are ordered from big to small volume or
   surface.
-  **Pattern along the positive X-axis:** See image below.
-  **Pattern along the negative X-axis:** See image below.
-  **Pattern along the positive Y-axis:** See image below.
-  **Pattern along the negative Y-axis:** See image below.
-  **Highest matching score (Teach only):** Sort objects with the
   highest model matching score first. This only works for the Teach
   detection

The pattern sort options are useful for depalletization or pallet
loading applications. The picture below illustrates each option:

|image4|

.. |image0| image:: https://s3.amazonaws.com/helpscout.net/docs/assets/583bf3f79033600698173725/images/5abdf8b1042863794fbec1ac/file-66mLZ7pD5u.png
.. |image1| image:: https://s3.amazonaws.com/helpscout.net/docs/assets/583bf3f79033600698173725/images/5ac4f2df2c7d3a0e936702d1/file-YSiaSfA2dA.png
.. |image2| image:: https://s3.amazonaws.com/helpscout.net/docs/assets/583bf3f79033600698173725/images/5ac4f06c2c7d3a0e936702bf/file-AIyAGwz6XG.png
.. |image3| image:: https://s3.amazonaws.com/helpscout.net/docs/assets/583bf3f79033600698173725/images/5abe4d19042863794fbec35b/file-yDidTrHTbG.png
.. |image4| image:: https://s3.amazonaws.com/helpscout.net/docs/assets/583bf3f79033600698173725/images/5ac4f3592c7d3a0e936702d9/file-GQShjfESmv.png

