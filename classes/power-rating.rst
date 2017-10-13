Power Ratings
=============

When an `ability <ability.html>`_ is executed, there can be certain environmental factors which affect its performance. For example, a full moon would enhance waterbending power. The *power rating* represents a numerical score of the current environment.

The power rating itself is a :code:`double` value between -100 and +100. The higher the power rating, the better the environment is rated (and vice versa, for negative values). Zero represents no change in the environment.

The power rating is calculated by a combination of *modifiers*. Each modifier represents a certain condition, and either adds or subtracts from the total power rating. For example, a full moon would add a FullMoonModifier, which could increase the power modifier by 30 points. Multiple modifiers can be present at the same time.

Modifiers
---------

A power rating modifier adds or subtracts from total power rating. This value is calculated based on a given :code:`BendingContext` object, which holds information about the current situation. The value of the modifier is found using the :code:`PowerRatingModifier#get` method, which is called every time the power rating is calculated.

Modifiers usually only exist for a short amount of time. Their :code:`onUpdate` method is called every tick, and if the method returns true, they remove themselves. By default, power modifiers remove themselves after a specified number of ticks (changed by the :code:`setTicks` method), but subclasses can override :code:`onUpdate` and customize when to remove the modifier.

Modifiers are not singletons; a new instance of a modifier subclass is created every time that modifier should be applied.

