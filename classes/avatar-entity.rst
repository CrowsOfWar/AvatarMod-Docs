Avatar Entities
---------------

Many `abilities <ability.html>`_ create an entity as part of the move; for example, the fireball ability creates a fireball entity that's manipulated by the bender. All entities from abilities derive from the :code:`AvatarEntity` class. Please note, animals and other mobs added by the mod extend from vanilla classes (such as :code:`EntityAnimal`).

AvatarEntities all have an owner, which refers to who created the entity (using bending). They also can sometimes have a _controller_, which is who is currently controlling the movement of the entity. The controller is null if the entity isn't being controlled by anyone right now. Depending on the current state, an entity may have an owner but not have a controller (for example, a thrown floating block).

Unlike vanilla Entities, AvatarEntities integrate better with `avatarmod vectors <vector.html>`_. They have methods :code:`velocity()` and :code:`setVelocity(vec)` to get and set velocity in m/s, and :code:`position()` and :code:`setPosition(vec)` to get and set position.

Also unlike vanilla Entities, you can send references to an AvatarEntity over network. Vanilla entity Ids aren't synchronized between server and client, so they can't be used to synchronize entities. AvatarEntities have their own, synchronized Id which is called the AvId. It can be used by calling :code:`getAvId()`. AvatarEntities can also be looked up by Id through the method :code:`AvatarEntity.lookupEntity(world, id)`.
