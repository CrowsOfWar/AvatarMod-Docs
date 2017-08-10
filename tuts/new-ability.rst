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

Knowledge of the `ability data class <../classes/ability-data.html>`_ is necessary. An instance of ability data can be obtained through :code:`AbilityContext#getAbilityData()`. Just a few method calls to adjust the ability numbers can be sufficent:

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
     


TODO; use Chi, award Xp, base on Level, support paths

Localization
------------

TODO; add translation and desc for each upgrade

Icon
----

TODO; add png to correct assets folder

AI
---

TODO
