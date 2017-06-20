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
