Bender interface
================

In AV2, there are many types of benders. Players, mobs, and even other possibilites (statues?) can access bending abilities. The bender interface exists to abstract behavior so it can be applied to players or mobs.

Features that the Bender interface provides includes:

- Access to bending data
- Convenient methods like `isPlayer()`
- Logic that varies based on player/mob, like `consumeWaterLevel(amount)`
  - players have water in their pouches
  - mobs have infinite water
- Way to identify benders (see BenderInfo)

A Bender object can be obtained for an entity by calling `Bender#create(EntityLivingBase)`. Only some types of entities can be Benders. It will throw an exception if Bender is not supported for that type of entity.

Implementations
---------------

The `Bender` interface is implemented in the following ways:

- For players, `PlayerBender` wraps a player entity and returns results based on that
- Current mobs directly implement the Bender interface - see superclass `EntityBender`

BenderInfo
----------

Oftentimes a way to find a Bender by uuid/name is necessary. It also is good to have it abstracted from whether it is a player or not. BenderInfo allows you to identify Benders (either players or mobs) by using their UUID.

- BenderInfo finds the Bender by iterating through the world's loaded entities, and creating a new bender object off of that player
- In the case of players, uses account's UUID
- In the case of mobs, uses its randomly generated UUID

There is a subclass called `NoBenderInfo` which represents no bender found. This uses the null object pattern and can prevent NPEs.

BenderInfo is written to disk using the `writeToNbt` method, and can be read by calling the static method `readFromNbt`. It is written and read from network via the BenderInfo serializer found in `AvatarDataSerializers`.

Cookbook
--------

Check if an entity (which implements Bender) is currently flying

.. code-block:: java

  EntityLivingBase entity = /* ... */;
  Bender bender = Bender.create(entity);
  boolean flying = bender.isFlying();

Store information about a Bender to disk so it can be accessed later

.. code-block:: java

   NBTTagCompound nbt = /* ... */;
   Bender bender = /* ... */;
   BenderInfo info = bender.getInfo();
   info.writeToNbt(nbt);

Read the BenderInfo and create a new Bender

.. code-block:: java

   NBTTagCompound nbt = /* ... */;
   World world = /* ... */;
   BenderInfo info = BenderInfo.readFromNbt(nbt);
   Bender bender = info.find(world); // can be null if Bender not found
