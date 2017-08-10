Tutorial: New Ability
=====================

This tutorial will show you how to add a new `ability <../classes/ability.html>`_ to the mod and attach it to a `bending style <../classes/bending-style.html>`_. The ability in this example will be a Firebending ability that lights the targeted mob on fire.

Basic Setup
-----------

To make the new ability, extend the Ability class. You need to call the super constructor, which takes the name of the ability. Also the abstract method :code:`execute` needs to be implemented - leave it blank and fill it in later.

.. code-block:: java
   
   public class AbilityImmolate extends Ability

You'll need to register the ability to the `ability registry <../classes/ability.html#registry>`_ so it's added to the game. You also need to `add it to the bending style <../classes/bending-style.html#registry>`_. This step is rather easy.

In :code:`AvatarMod.registerAbilities()`, call :code:`Abilities.register` with a new instance of your ability. Then call :code:`BendingStyles.addAbility` to attach the ability to the bending style.

.. note:: If you're developing an add-on, just call these methods in your main mod class's :code:`postInit` method.
   If your main mod class doesn't have a postInit, just add one which has the method header :code:`@EventHandler public void postInit(FMLPostInitializationEvent e)`. FML will automatically call it, similar to how preInit is called.

The ability is now registered and visible in the Firebending radial menu.

Execute method
--------------

Now, it's time to fill in the :code:`execute` method. It is called whenever the ability is used, only on the server side. This is where you define the ability's functionality.

This ability will light the targeted entity on fire. The basic logic flow will be to use `a raytrace <../classes/raytrace.html>`_ to find the targeted mob and then set it on fire.

Here is an example implementation of that ability.

.. code-block:: java
   
   @Override
   public void execute(AbilityContext ctx) {
     final double range = 5;
     final float fireSeconds = 4;     

     World world = ctx.getWorld();
     EntityLivingBase entity = ctx.getBenderEntity();
     
     Vector start = Vector.getEyePos(entity);
     Vector dir = Vector.getLookRectangular(entity);
     List<Entity> raytrace = Raytrace.entityRaytrace(world, start, dir, range);
     
     // only immolate 1 entity
     if (!raytrace.isEmpty()) {
       raytrace.get(0).setFire(fireSeconds);
     }
     
   }

Here are links to detailed docs on each class used:

- `AbilityContext <../classes/abilityctx.html>`_
- `Vector <../classes/vector.html>`_
- `Raytrace <../classes/raytrace.html>`_

Improving the implementation
----------------------------

Although the ability works, there are still a lot of missing features common to other abilities. The ability doesn't require chi, so can be spammed continuously. It ignores experience and level 4 options, and therefore has no progression. Fortunately, adding these features are pretty simple.

Applying Xp cost is super easy. Just override the super method :code:`getChiCost()`
and return the correct amount of chi. To require more chi under other conditions
(such as air bubbles consuming chi each second), see :code:`Bender#consumeChi`.

Adding experience only requires a few method calls, but knowledge of the `ability data class <../classes/ability-data.html>`_ is necessary. An instance of ability data can be obtained through :code:`AbilityContext#getAbilityData()`. Simply adjusting the numbers can be sufficent:

.. code-block:: java
   
   @Override
   public void execute(AbilityContext ctx) {
     AbilityData abilityData = ctx.getAbilityData();
     double range = 3 + abilityData.getLevel() * 0.5;
     float fireSeconds = 2 + abilityData.getLevel();
     
     if (abilityData.isMasterPath(AbilityTreePath.FIRST)) {
       range = 8;
       fireSeconds = 3;
     }
     if (abilityData.isMasterPath(AbilityTreePath.SECOND)) {
       range = 3;
       fireSeconds = 7;
     }

     // etc...
   }

To award Xp, just call :code:`abilityData.addXp` when appropriate.

Localization
------------

For this ability, the following translations will be necessary:

- Name of the ability: :code:`avatar.ability.immolate`
- Description of the ability: :code:`avatar.ability.immolate.desc`
- Description of each level upgrade:
  :code:`avatar.ability.immolate.lvlX` (see below)

For levels the following levels are used:

- :code:`lvl1` - level I
- :code:`lvl2` - level II
- :code:`lvl3` - level III
- :code:`lvl4_1` - level IV, first path
- :code:`lvl4_2` - level IV, second path

Simply add the keys to en_US.lang, for example:

.. code-block::
   
   avatar.ability.immolate.desc=You direct your fire to burn on organic material. Sets the targeted enemy on fire for a few moments.

Icon
----

There are a few icons needed, one for radial menu, one for the skills menu card, and
one for the skills menu background.

For the radial menu icon, use a 256x256 image and name it :code:`icon_immolate.png`.
Place it in the :code:`textures/radial` folder of the assets.

For the skill menu card and skill menu background, don't bother because by the time
someone else is reading this article, I'll have scrapped that approach as EduMC can't make
skill menu pictures anymore.

AI
---

The ability is almost all set up - functionality, UI, translations... The only thing
left is an Ai module, which tells bending mobs how to use this ability (this part is
optional if bending mobs shouldn't use the ability).

For starters, override the method :code:`getAi`, and see the :code:`AiWaterArc` class
for an example. Bending Ai is not covered in this tutorial; detailed documentation is available `here <../classes/bending-ai.html>`_.
