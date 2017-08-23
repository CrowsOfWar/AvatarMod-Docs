Bending data
============

Information about `benders' <bender.html>` progress is stored in BendingData instances. This information includes unlocked `bending styles <bending-style.html>`, `ability data <ability-data.html>`, and more. Information here is persistent and synchronized between client and server.

Retrieval
---------

A :code:`BendingData` object can be obtained by calling the following methods:

- :code:`BendingData.get(entity)` - gets data for that entity, if it's a bender
- :code:`BendingData.get(world, accountId)` - gets data for the player who has that `account Id <account-uuids.html>`. The player can be offline.
- :code:`BendingData.get(world, username)` - gets data for the player who has that username. The player can be offline.

If you have a `Bender <bender.html>` object, you can also use :code:`Bender#getData()`.

Bending Styles
--------------

There are many different methods to use the large amount of data contained in BendingData.

Some useful methods relevant to the current unlocked bending styles include:

- :code:`hasBending(UUID)` - whether the bending style with that Id is unlocked
- :code:`addBending(UUID)` - unlocks the bending style with the given Id
- :code:`removeBending(UUID)` - revokes the bending style with the given Id
- :code:`getAllBendingIds()` - gets a list of the Ids of the currently unlocked bending styles

There are also overloads to avoid using a BendingStyle's Id as parameters and pass a BendingStyle instance instead. This can be convenient, but keep in mind a BendingStyle `may not currently be present <../offline.html>` so these methods should only be used when you know the BendingStyle isn't null.

Ability data
------------

There are also methods pertaining to `ability data <ability-data.html>`. Ability data keeps track of a specific ability's level, Xp progress, and unlocked status.

The most relevant method for dealing with ability data is :code:`getAbilityData(UUID)`, which gets ability data for the ability with that Id. As with methods for BendingData, there's an overload to pass in an actual :code:`Ability` instance, but this should only be used in code where the ability is guaranteed to be loaded.

There are a few other methods for ability data, but this is the only useful one 90% of the time.

Status Controls
---------------

`Status controls <status-control.html>` are temporary key listeners that execute some code upon being pressed. (keeping StatusControls in synchronized BendingData is necessary because SCs need persistence)

Here are a few methods to access status controls:

- :code:`hasStatusControl(StatusControl)` - whether the status control is present
- :code:`addStatusControl(StatusControl)` - add the status control
- :code:`removeStatusControl(StatusControl)` - remove the status control
- :code:`getAllStatusControls()` - get all current status controls

Tick Handlers
-------------

Data can have multiple `tick handlers <tick-handler.html>`. As their name implies, a tick handler is an object which recieves updates every tick. Any tick handler in the BendingData will recieve updates if the Bender is online.

The method :code:`addTickHandler(TickHandler)` activates the tick handler. An instance of a tick handler is found from a public static variable instance in the :code:`TickHandler` class.

BendingData keeps track of how long a TickHandler was used for. This can be accessed using :code:`getTickHandlerDuration(TickHandler)`, which represents the time in ticks. The time isn't synchronized between the server and client, and isn't saved to disk, so avoid using this timer for anything other than quick, short-term delays.

Miscellaneous 
-------------

Some extra information is stored in BendingData. This information isn't very complex, but needs to be persistent so is stored in BendingData. One example is bison follow mode: whether tamed flying bison should follow the player. These bits of information are found from :code:`getMiscData()` but there are delegates to MiscData in the BendingData class.
