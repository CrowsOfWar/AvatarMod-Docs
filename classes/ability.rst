Ability Class
=============

The :code:`Ability` class represents an ability that `benders <bender.html>`_ can use. Abilities are singletons; they don't contain any information specific to a particular bender (`AbilityData is used for this <ability-data.html>`_). Information common to all abilities include:

- A name
- The `bending style <bending-style.html>`_ this ability belongs to
- Functionality (i.e., the :code:`execute` method)
- Misc. other information, such as chi cost

More aspects of the ability like `an entity <avatar-entity.html>`_ are included only in some abilities.


