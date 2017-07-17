Bender interface
================

In AV2, there are many types of benders. Players, mobs, and even other possibilites (statues?) can access bending abilities. The bender interface exists to abstract behavior so it can be applied to players or mobs. A :code:`Bender` object only represents a Bender that is currently in the world (i.e. excluding offline players).

Features that the Bender interface provides includes:

- Access to bending data
- Convenient methods like :code:`isPlayer()`
- Logic that varies based on player/mob, like :code:`consumeWaterLevel(amount)`
  - players have water in their pouches
  - mobs have infinite water
- Way to identify benders (see BenderInfo)

A Bender object can be obtained for an entity by calling :code:`Bender#create(EntityLivingBase)`. Benders are to be used on both logical sides.

Only some types of entities can be Benders. It will throw an exception if Bender is not supported for that type of entity.

Implementations
---------------

The :code:`Bender` interface is implemented in the following ways:

- For players, :code:`PlayerBender` wraps a player entity and returns results based on that
- Current mobs directly implement the Bender interface - see superclass :code:`EntityBender`

BenderInfo
----------

The :code:`Bender` interface describes a Bender which is present in the world, but sometimes the bender must be saved in various scenarios:

- Ability to identify offline players
- Saving information tied to offline players
- Sending Benders across the network (the :code:`Bender` cannot be directly sent because there are different instances of that :code:`Entity` on server/client)

BenderInfo allows you to identify Benders (either players or mobs) by using their UUID. It can be used both on the server and client.

Internally, BenderInfo uses several subclasses to simplify implementation. Therefore, you cannot instantiate a BenderInfo directly. There are several ways to get a BenderInfo object:
- :code:`Bender#getInfo()` - get BenderInfo about a particular bender
- :code:`BenderInfo#get(EntityLivingBase entity)`  - get BenderInfo for a particular entity, provided that it is a Bender
- :code:`BenderInfo#readFromBytes(ByteBuf) or BenderInfo#readFromNbt(NBTTagCompound)` - read BenderInfo from disk or network. There are also writing methods that you can call once you have a BenderInfo instance.
- :code:`BenderInfo#get(boolean player, UUID id)` - get BenderInfo using your own parameters. This isn't recommended as there are simpler ways to do it (see above)

- BenderInfo finds the Bender by iterating through the world's loaded entities, and creating a new bender object off of that player
- In the case of players, uses `account's UUID <account-uuids.html>`_
- In the case of mobs, uses its randomly generated UUID

There is a subclass called :code:`NoBenderInfo` which represents no bender found. This uses the null object pattern and can prevent NPEs.

BenderInfo is written to disk using :code:`writeToNbt`, and can be read by calling the static method :code:`readFromNbt`. It is written and read from network via the BenderInfo serializer found in :code:`AvatarDataSerializers`.

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
