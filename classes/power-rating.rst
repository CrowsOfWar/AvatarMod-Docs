Power Ratings
=============

When an `ability <ability.html>`_ is executed, there can be certain environmental factors which affect its performance. For example, a full moon would enhance waterbending power. The *power rating* represents a numerical score of the current environment.

The power rating itself is a :code:`double` value between -100 and +100. The higher the power rating, the better the environment is rated (and vice versa, for negative values). Zero represents no change in the environment.

The power rating is calculated by a combination of *modifiers*. Each modifier represents a certain condition; for example a full moon would add a FullMoonModifier, which would increase the power modifier. Multiple modifiers are usually present at the same time.
