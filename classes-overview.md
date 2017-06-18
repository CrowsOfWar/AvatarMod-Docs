Classes overview
================

This page explains the most important classes and interfaces used in AvatarMod that are necessary for basic understanding of the code.

Bending data
------------

There needs to be a place to store information about players' bending, progress, and other information. It also must be compatible with mobs and other entities, which means data should be tied to a `Bender` instead of a player entity. The interface `BendingData` (`com.crowsofwar.avatar.common.data.BendingData`) provides the framework for data.
