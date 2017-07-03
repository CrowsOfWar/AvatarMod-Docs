Ability data
============

Any ability data is stored in the `AbilityData` class. Each AbilityData keeps track of a Bender's progress while learning a specific ability, for example:

- ability level
- experience to next level
- upgrades

There is one instance of AbilityData created per ability, per `BendingData <bending-data.html>`.

An instance of AbilityData can be retrieved by calling `BendingData#getAbilityData(BendingAbility)`.

Cookbook
--------

Check if an ability is unlocked

.. code-block:: java

   BendingData data = Bender.getData(player);
   AbilityData abilityData = data.getAbilityData(ability);
   boolean unlocked = !abilityData.isLocked();

Add 10% XP. Won't advance to the next level.

.. code-block:: java

   BendingData data = Bender.getData(player);
   AbilityData abilityData = data.getAbilityData(ability);

   // Don't use addXp since the number would be adjusted for XP decay
   abilityData.addXpDirect(10);
