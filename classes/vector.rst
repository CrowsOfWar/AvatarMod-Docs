Vector
======

A :code:`Vector` object represents an immutable, three-dimensional mathematical vector. Vectors can represent many things, such as velocity or position.

.. warning:: This refers to the class :code:`com.crowsofwar.gorecore.util.Vector` class, not in any related openGL or JOML packages

Basic Usage
-----------

Vectors can be created using the appropriate constructor, such as :code:`new Vector(x, y, z)`. Their coordinates can be retrieved via the :code:`x()`, :code:`y()`, and :code:`z()` methods.

Vectors have many different methods to perform basic computations. As a vector is immutable, these methods create a new vector based on the old one and the parameters. Methods use the `fluid interfaces <https://en.wikipedia.org/wiki/Fluent_interface#Java>`_ naming convention. For example, addition is performed via the :code:`plus` method.

Method chaining is encouraged. Consider the following code:

.. code-block:: java
   Vector motion = /* ... */;
   motion = motion.times(2);
   motion = motion.plusY(1);
   motion = motion.times(-1);

The same could be accomplished much more concisely via method chaining:

.. code-block:: java
   Vector motion = /* ... */;
   motion = motion.times(2).plusY(1).times(-1);

Trigonometry
------------

There are also several methods that deal with physics and trigonometry.

* To convert from and to spherical (polar) coordinates, use :code:`Vector#toSpherical()` or :code:`Vector#toRectangular()`
* To get rotations between vectors, use :code:`Vector#getRotationTo(from, to)`
* To reflect vectors off a normal, use :code:`Vector#reflect(normal)`

Rotations
---------

For vectors which represent rotation (Euler angles), the y-component represents yaw and the x-component represents pitch. The z-component is unused as Minecraft doesn't support roll. Methods will say whether the rotation is in degrees or radians.

Minecraft integration
---------------------

Vectors were designed to be used in a Minecraft environment, so there are some methods which allow convenient usage of Vectors with entities.

* To get an entity's position, use :code:`Vector#getEntityPos(entity)`. To get their eye position, use :code:`Vector#getEyePos(entity)`
* To get an entity's motion, use :code:`Vector#getVelocity(entity)`. The velocity is in meters per second (20 ticks).
* To get the direction in which an entity is looking in rectangular coordinates, use :code:`Vector#getLookRectangular(entity)`
* To convert to a BlockPos, use :code:`Vector#toBlockPos()`
* To find the direction to lob a projectile, use :code:`Vector#getProjectileAngle(velocity, gravity, horizontalDist, verticalDist)`

Cookbook
--------

Find the direction to other location

.. code-block:: java

   Vector us = /* ... */;
   Vector them = /* ... */;
   Vector directionTo = Vector.getRotationTo(us, them);
   double yaw = directionTo.y();
   double pitch = directionTo.x();

Check if two positions are within 10 blocks of each other

.. code-block:: java

   Vector pos1 = /* ... */;
   Vector pos2 = /* ... */;
   // Use Square Distance here to avoid a sqrt operation
   // Sqrt is expensive so avoid it if possible
   double distSq = pos1.sqrDist(pos2);
   boolean within10Blocks = distSq <= 10 * 10; // if distance is squared, need to square other side as well

Find the position of where the player is looking, 3 blocks ahead

.. code-block:: java

   EntityPlayer player = /* ... */;
   Vector position = Vector.getEyePos(player); // use getEyePos not getEntityPos
   Vector look = Vector.getLookRectangular(player);
   Vector lookPosition = position.plus(look.times(3)); // using .plus() here since .add() would modify position
   // Note: this shouldn't be used for calculating the block player is looking at
   //  since it doesn't take into account any blocks in the way of the player
   // For that purpose, use Raytrace instead
