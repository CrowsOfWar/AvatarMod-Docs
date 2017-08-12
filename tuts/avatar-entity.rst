Creating Avatar Entities
========================

This tutorial explains how to create a new `AvatarEntity <../classes/avatar-entity.rst>`_. AvatarEntities are used when an ability creates a new entity, such as the Fireball ability making a fireball entity.

Keep in mind that although AvatarEntities add many new APIs, they are still very similar to vanilla entities. The features of an AvatarEntity are for convenience, and depending on the specific ability you might only use some API features. Therefore, this guide will be much "looser" than most tutorials.

Creation
--------

This part is very straightforward and very similar to creating vanilla entities. Make a class extending :code:`AvatarEntity` and implement the constructor. Then, register it in :code:`AvatarMod#registerEntities`.

Override the :code:`getController` method to return who's in control of this entity. For some entities, players can't control the entity so this will always return null (e.g. a ravine). For others, players can sometimes control the entity (such as a floating block).

Basic Features
--------------

Use :code:`getOwner()` and :code:`setOwner(owner)` to access the owner of the entity. When you create the entity in the ability class, make sure you call :code:`setOwner`.

AvatarEntities that wish to handle collisions should do so in the :code:`onCollideWithEntity` method.

Common Operations
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
