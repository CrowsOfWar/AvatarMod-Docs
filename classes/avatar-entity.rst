Avatar Entities
---------------

Many `abilities <ability.html>`_ create an entity as part of the move; for example, the fireball ability creates a fireball entity that's manipulated by the bender. All entities from abilities derive from the :code:`AvatarEntity` class. Keep in mind AvatarEntity is _only_ for ability related entities; mobs extend from other classes like :code:`EntityAnimal`.

AvatarEntities all have an owner, which refers to who created the entity (using bending). Under normal conditions, the owner should always be set. They also sometimes have a _controller_, which refers to who currently controls the movement of the entity. Sometimes nobody is manipulating the entity's movement, so the controller may not be set.

Nullability
-----------

Keep in mind that although the owner or controller of an AvatarEntity might be set, they may not be logged in right now so an attempt to get the entity instance will return null.

Vectors
-------

Unlike vanilla Entities, AvatarEntities integrate better with `avatarmod vectors <vector.html>`_. Although vectors can already be used with vanilla entities, there are convenient methods to use Vectors for AvatarEntities:

- Use :code:`position()` and :code:`setPosition(vec)` to manipulate the entity's position
- Use :code:`velocity()` and :code:`setVelocity(vec)` to manipulate the entity's velocity. Velocity is in meters per second.

AvId
----

Also unlike vanilla Entities, you can send references to an AvatarEntity over network. Vanilla entity Ids aren't synchronized between server and client, so they can't be used to synchronize entities. AvatarEntities have their own, synchronized Id which is called the AvId. It can be used by calling :code:`getAvId()`. AvatarEntities can also be looked up by Id through the method :code:`AvatarEntity.lookupEntity(world, id)`.
