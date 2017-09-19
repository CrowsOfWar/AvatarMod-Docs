Ability data
============

Any ability data is stored in the `AbilityData` class. Each AbilityData keeps track of a Bender's progress while learning a specific ability, for example:

- ability level
- experience to next level
- upgrades

There is one instance of AbilityData created per ability, per `BendingData <bending-data.html>`_.

An instance of AbilityData can be retrieved by calling :code:`BendingData#getAbilityData(BendingAbility)`.

Common Methods
--------------

Here are most of the methods you will be using in AbilityData:

- :code:`#getLevel()` - returns the current level of the ability. The levels **start at 0**, so a value of 0 would represent level I, etc.
- :code:`#isMasterPath(AbilityTreePath)` - checks if the ability is at level IV and on that upgrade choice. The upgrade path can either be :code:`AbilityTreePath.FIRST` or :code:`AbilityTreePath.SECOND`.
- :code:`#addXp(float)` - adds the given amount of experience to the ability data. Keep in mind, at higher levels players get diminishing returns for the same amount of experience. This means if you grant, say, 10 XP, it might grant less than 10 XP to make it harder for the player to level up.

Cookbook
--------

Check if an ability is unlocked

.. code-block:: java

   EntityPlayer player = /* ... */;
   UUID abilityId = /* ... */;
   AbilityData abilityData = AbilityData.get(player, abilityId);
   boolean unlocked = !abilityData.isLocked();

Add 10% XP. Won't advance to the next level.

.. code-block:: java

   BendingData data = /* ... */;
   Ability ability = /* ... */;
   AbilityData abilityData = data.getAbilityData(ability.getId());

   // Don't use addXp since the number would be adjusted for XP decay
   abilityData.addXpDirect(10);
