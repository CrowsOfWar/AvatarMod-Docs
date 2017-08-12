Creating Avatar Entities
========================

This tutorial explains how to create a new `AvatarEntity <../classes/avatar-entity.rst>`_.

Collisions
----------

AvatarEntities that wish to handle collisions should do so in the :code:`onCollidieWithEntity` method.

Common operations
-----------------

There are many operations that multiple AvatarEntities need to perform. These often are relatively simple yet involve a significant amount of code; for example breaking a block also involves dropping items, playing sounds, and creating particles. Several protected methods are available to simplify these operations:

- :code:`breakBlock(pos)` - break a block at the specified BlockPos
- :code:`spawnExtinguishIndicators()` - plays effects to indicate something is extinguished

Cookbook
--------

Extinguish when hit water

.. code-block:: java

   @Override
   public boolean onLargeWaterContact() {
     spawnExtinguishIndicators();
     setDead();
     return true;
   }reating Avatar Entities
========================

The below information only is useful if you are editing a class extending AvatarEntity.

Collisions
----------

AvatarEntities that wish to handle collisions should do so in the :code:`onCollidieWithEntity` method.

Common operations
-----------------

There are many operations that multiple AvatarEntities need to perform. These often are relatively simple yet involve a significant amount of code; for example breaking a block also involves dropping items, playing sounds, and creating particles. Several protected methods are available to simplify these operations:

- :code:`breakBlock(pos)` - break a block at the specified BlockPos
- :code:`spawnExtinguishIndicators()` - plays effects to indicate something is extinguished

Cookbook
--------

Extinguish when hit water

.. code-block:: java

   @Override
   public boolean onLargeWaterContact() {
     spawnExtinguishIndicators();
     setDead();
     return true;
   }
