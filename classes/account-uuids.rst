Account UUIDs
=============

Minecraft stores data about players by a :code:`UUID`, which is unique per account. The reason Minecraft doesn't store information by username is because people can change it. If data was stored by username, and a user changed their username, all data about them would be deleted.

The :code:`AccountUUIDs` class provides easy access to players' UUIDs by using the Mojang API. All methods in this class are static. You can always get a player's UUID even when they are offline.

Basic usage
-----------

To get a player's UUID from their username, use :code:`AccountUUIDs#getId(username)`. To get a player's username from their UUID, use :code:`AccountUUIDs#getUsername`.

.. warning::
  When you have an :code:`EntityPlayer` object and need to access their account UUID, don't use :code:`player.getUniqueId()` or :code:`player.getPersistentId()`. These are generic entity Ids and don't actually refer to the account Id. Instead, use AccountUUIDs: :code:`AccountUUIDs.getId(player.getName())`.

AccountId
---------

In some cases, AvatarMod may not be able to get an account UUID for the player:

* Problems with the internet
* Invalid/cracked account (username not registered)

When this happens, a "fake" UUID is generated based on the username; this allows the player to keep playing although the UUID is not the offical one from Mojang. This UUID is considered to be temporary. When Minecraft is restarted, AccountUUIDs will try to access the Mojang API again and get the offical UUID.

How does that related to the :code:`AccountId` class? Well, this class is needed to keep track of whether a UUID is temporary or not, and therefore whether the Mojang API needs to be contacted again to try to get an official UUID.

The account UUID can be accessed by calling :code:`AccountId#getUUID()`, and you can check if it is temporary by calling :code:`AccountId#isTemporary()`.

Caching
-------

To improve performance and reduce unneeded API calls, the AccountIds are cached both in a map at runtime, and on disk in a text file.

Cookbook
--------

Get a player entity's account UUID

.. code-block:: java

   // do NOT use player.getUniqueId() or player.getPersistentId()
   // these don't refer to the account's UUID
   EntityPlayer player = /* ... */;
   AccountId accountId = AccountUUIDs.getId(player.getName());
   UUID uuid = accountId.getUUID();

Get a username based on account UUID

.. code-block:: java

   UUID uuid = /* ... */;
   String username = AccountUUIDs.getUsername(uuid);
