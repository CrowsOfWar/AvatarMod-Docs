Classes overview
================

This page explains the most important classes and interfaces used in AvatarMod that are necessary for basic understanding of the code.

.. note::

   There is a full list of all documented classes `here <class-list.html>`_.

Bending data
------------

*Main article: `BendingData <../classes/bending-data.html>`_*

There needs to be a place to store information about players' bending, progress, and other information. It also must be compatible with mobs and other entities, so data is tied to a :code:`Bender` (more information below). The :code:`BendingData` class provides the framework for such data. To maintain offline support, it is detached from the entity so you can access offline players' data.

Data is fully synchronized across server and client. Whenever data is changed, a packet is sent to the client containing the new data, so you won't have to worry about manually synchronizing data.

BendingData can be obtained by using :code:`BendingData#get(EntityLivingBase)`. It is also obtained by using :code:`Bender#getData()` (see below).

Code example which toggles whether bison follow the player:

.. code-block:: java

   EntityPlayer player = /* ... */;
   BendingData data = BendingData.get(player);
   data.setBisonFollowMode(!data.getBisonFollowMode());

Bender
------

*Main article: `Bender <../classes/bender.html>`_*

Data and logic needs to not only apply to players, but also to mobs and other things. For example, abilities are compatible with both players and firebender mobs. For this to happen, there needs to be an abstraction. The :code:`Bender` interface detaches logic from traditional :code:`EntityPlayer` oriented code. Benders can represent mobs, players, or whatever else the implementer wishes. There are different implementations of this interface for players and mobs. (`main article <classes/bender.html>`_)

Since a :code:`Bender` object represents an in world mob/player, usually access to an :code:`Entity` is required to get position, motion, and other data. :code:`Bender` objects must provide an :code:`EntityLivingBase` which represents the entity version of the Bender. A player bender would provide an instance of :code:`EntityPlayer`, while a firebender would provide an instance of :code:`EntityFirender`. To maintain offline support, :code:`Bender#getEntity()` can return null if the Bender is not currently in the world. This usually happens when there is a command being used about an offline player.

To obtain an instance of a Bender for your entity, call the static method :code:`Bender.create(entity)`.

Bending Styles and Abilities
----------------------------

*`BendingStyles main article <../classes/bending-style.html>`_*

BendingStyles represent each style of bending; mainly all of the abilities but also miscellaneous information about them (such as name). All bending controllers are derived from the :code:`BendingStyle` class. The :code:`BendingStyles` class also keeps instances and of each bending style.

BendingStyles' main job is to keep track of a player's abilities.

*`Abilities main article <../classes/ability.html>`_*

The :code:`Ability` class represents an ability in-game. Its main purpose is to implement the :code:`execute` method, which causes a player to execute the ability.
