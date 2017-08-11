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

Similar to `BendingStyles <bending-style.html#Registry>`_, the :code:`Abilities` class keeps track of all the abilities. In this class, all methods are static so you never should create an instance of Abilities.

An ability instance can be obtained by calling :code:`Abilities.get(abilityName)`. Keep in mind that the ability can be null (explained below).

When `creating new abilities <../tuts/new-ability.html>`_, the new ability needs to be registered. This can be done by calling :code:`Abilities.register(myability)` where :code:`myability` is a new instance of your ability (remember, abilities are singletons).

There are several other methods in this registry, notably :code:`Abilities.all()` which returns a list of all abilities.

Offline
-------

From the Registry section, you may have noticed abilities can sometimes return null, and you need to handle for that. This is to avoid issues in scenarios where ability add-ons are uninstalled. For example, consider this scenario:

- A player installs an add-on which adds ability X
- The player creates data which references ability X
- The player decides to remove the add-on

At this point, data exists about ability X, but ability X is no longer present in the current installation. If the code can't handle an ability that isn't present, there is a good chance of fatal NPEs. The code must be able to handle that an ability isn't present (in this case, ignore data about ability X). If the user re-installs that add-on, the data would still be preserved thanks to the code's tolerance for a missing ability.

(A comparison of this approach to Forge's approach: A similar situation happens when a user uninstalls a mod that contained new items. Instead of gracefully ignoring these missing items from the world, Forge will actually delete all those items. Although it's a little easier to implement in the code, this is much worse for the user.)

To account for this problem, simply avoid using an actual instance of :code:`Ability` unless you are *sure* it is loaded. Otherwise, handle using the ability's name.

**tl;dr** Opt for using ability's name instead of ability instance whenever possible

Naming
------

As explained above, abilities will often be referred to by their names. Although ability naming does not have any strict rules in place, they still should follow several conventions.

For consistency, names should be all lowercase, with underscores separating different words. 

For add-ons, ability names should be namespaced to avoid naming conflicts. Follow vanilla Minecraft's ResourceLocation convention: the add-on name, followed by a colon, followed by the ability name. Ability names from the core AvatarMod need not follow this rule; they only contain the ability name (without :code:`avatarmod:`).

Examples:

- AvatarMod ability "Light Fire": :code:`light_fire`
- Add-on ability "Plasma Blast": :code:`addon:plasma_blast`

Cookbook
--------

(Note: there is `a tutorial <../tuts/new-ability>`_ for how to create new abilities)

Find out which bending style an ability belongs to

.. code-block:: java
   
   String abilityName = /* ... */;

   // Warning: ability can be null
   Ability ability = Abilities.get(abilityName);
   String bendingStyle = ability == null ? null : ability.getBendingStyle();

Have the `bender <bender.html>`_ use the fire arc ability (note: for AI, `BendingAi <bending-ai.html>`_ should be used, which also teaches the bender *how* to use that ability)

.. code-block:: java
   
   Bender bender = /* ... */;
   String abilityName = /* ... */;
   bender.executeAbility(abilityName);

For an Ai mob - adds the ability Ai to the task list, which tells the mob how to use that ability

.. code-block:: java
   
   @Override
   protected void initEntityAI() {
     int priority = /* ... */;
     String abilityName = /* ... */;

     BendingAi ai = Abilities.getAi(abilityName, this, this);
     // or...  ai = Abilities.get(abilityName).getAi(this, this)
     tasks.addTask(priority, ai);

     // ... rest of mob ai ...
   }


