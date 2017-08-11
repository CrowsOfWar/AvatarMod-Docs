Ability Class
=============

The :code:`Ability` class represents an ability that `benders <bender.html>`_ can use. Abilities are singletons; they don't contain any information specific to a particular bender (`AbilityData is used for this <ability-data.html>`_). :code:`Ability` instances are most important because they define the in-game ability's behavior.

An instance of an ability can be obtained with :code:`Abilities.get(abilityName)`, and abilities are executed by calling :code:`Bender.executeAbility(abilityName)`. Avoid directly using an Ability instance unless you are sure that ability is installed (`explanation <#Offline>`_).

Information common to all abilities include:

- A name
- The `bending style <bending-style.html>`_ this ability belongs to
- Functionality (i.e., the :code:`execute` method)
- Misc. other information, such as chi cost

(More aspects of the ability like `an entity <avatar-entity.html>`_ are included only in some abilities)

Registry
--------

The :code:`Abilities` class keeps track of all abilities and also allows retrieval of ability instances. Since tihs is like a registry, all methods are static and you never need to create an instance of Abilities. This is needed to avoid direct usage of an Ability instance (explained below).

An ability instance can be obtained by calling :code:`Abilities.get(abilityName)`. The ability can be null (see below), so keep that in mind to avoid NPEs.

When `creating new abilities <../tuts/new-ability.html>`_, the new ability needs to be registered. This can be done by calling :code:`Abilities.register(myability)` where :code:`myability` is a new instance of your ability (again, abilities are singletons).

There are several other methods in this registry, notably :code:`Abilities.all()` which returns a list of all abilities.

Offline
-------

From the Registry section, you may have noticed abilities can sometimes return null, and you need to handle for that. This is to avoid issues in the following scenario:

- A player installs an update/add-on which adds a new ability
- The player creates data which references that ability
- The player decides to remove the update/add-on

Now, data exists about an ability which is no longer present in the current installation. If the code can't handle an ability that isn't present (i.e. :code:`null`), there is a good chance of crashing or fatal bugs. The code must be able to handle that an ability isn't present (ignore the faulty data). And as a bonus, if the user re-installs that update or add-on, the data would still be preserved thanks to the code's tolerance for a missing ability.

A comparison of this approach to Forge's approach: A similar situation happens when a user uninstalls a mod that contained new items. Instead of gracefully ignoring these missing items from the world, Forge will actually delete all those items. Although it's a little easier to implement in the code, all those items would be lost so if the user reinstalled the mod, all his/her mod items would be gone.

To account for ability instances possibly not being present, simply avoid using an actual instance of :code:`Ability` unless you are *sure* it is loaded. Otherwise, handle using the ability's name.

**tl;dr** Opt for using ability's name instead of ability instance whenever possible
