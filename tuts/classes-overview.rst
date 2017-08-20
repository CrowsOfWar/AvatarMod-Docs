Classes overview
================

This page explains the most important classes and interfaces used in AvatarMod that are necessary for basic understanding of the code.

.. note::

   There is a full list of all documented classes `here <class-list.html>`_.

Bending data
------------

*Main article: `BendingData <../classes/bending-data.html>`_*

There needs to be a place to store information about players' bending, progress, and other information. It also must be compatible with mobs and other entities, so data is tied to a :code:`Bender` (more information below). The :code:`BendingData` class provides the framework for such data. To maintain offline support, it is detached from the entity so you can access offline players' data.

Bender
------

Data and logic needs to not only apply to players, but also to mobs and other things. For example, abilities are compatible with both players and firebender mobs. For this to happen, there needs to be an abstraction. The :code:`Bender` interface detaches logic from traditional :code:`EntityPlayer` oriented code. Benders can represent mobs, players, or whatever else the implementer wishes. There are different implementations of this interface for players and mobs. (`main article <classes/bender.html>`_)

Since a :code:`Bender` object represents an in world mob/player, usually access to an :code:`Entity` is required to get position, motion, and other data. :code:`Bender` objects must provide an :code:`EntityLivingBase` which represents the entity version of the Bender. A player bender would provide an instance of :code:`EntityPlayer`, while a firebender would provide an instance of :code:`EntityFirender`. To maintain offline support, :code:`Bender#getEntity()` can return null if the Bender is not currently in the world. This usually happens when there is a command being used about an offline player.

To obtain an instance of a Bender for your entity, call the static method :code:`Bender.create(entity)`. To obtain an instance of data, call :code:`Bender.create(entity).getData()`, or, more conveniently, :code:`Bender.getData(entity)`.

Data is fully synchronized across server and client. Whenever data is changed, a packet is sent to the client containing the new data, so you won't have to worry about manually synchronizing data.

Bending controllers
-------------------

Bending controllers represent each style of bending; mainly all of the abilities but also miscellaneous information about them (such as name). All bending controllers are derived from the :code:`BendingController` class. The :code:`BendingManager` class also keeps instances and IDs for each bending controller.

Bending controllers are stored in :code:`BendingData`. They are found via ID. Each ID can be retrieved through the appropriate static fields in :code:`BendingManager`, or by calling :code:`controller.getId()`.

Abilities
---------

The :code:`BendingAbility` class is the base for each ability. Obviously, the most important abstract method is :code:`execute(AbilityContext)`, where the code for the ability is executed. Instances of :code:`BendingAbility` are kept in static fields of the BendingAbility class itself. Their :code:`BendingController` objects also keep track of them.

Any logic in an ability (such as the :code:`execute` method) is only to be called on the server. Calling on client will result in issues like duplicate entities!
