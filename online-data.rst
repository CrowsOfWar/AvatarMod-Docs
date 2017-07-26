Online data
-----------

In this codebase, there are many different kinds of objects which might not exist at this time, but could exist in another runtime. Therefore, it's best to design in mind that a certain object might not exist during this runtime, and handle missing objects gracefully by *expecting* there to be a missing object.

For example, imagine one player installs an add-on to add an ability called RockArmor. The mod saves information associated with the RockArmor ability. Then, the player uninstalls the add-on and reloads the world. The mod loads information about the RockArmor ability, but a RockArmor Ability object doesn't exist anymore.
