Git Conventions
===============

The development of AvatarMod relies on `git <https://git-scm.com/>`_, which also provides valuable information about the project's tasks and history. For example, commit messages are an important summary of changes made.

To allow maximum readability of the git repository, you should follow these conventions.

.. note::
   
   Disclaimer: While we encourage you to follow these conventions, we haven't followed these rules perfectly 100% of the time.

Commit Messages
---------------

Commit messages provide a summary of a commit, and possibly reasoning behind the changes.

The first lines of commit messages (the "subject") should show a summary of the commit. At a glance, someone should know what the commit basically does.

The subject should be one sentence. It should be worded imperatively. This means instead of "The dog gets exercise", you would write "Walk the dog". Omit ending punctuation (.!?). Some good examples include:

- "Tweak turbulence values of 'furious' lightning"
- "Rename ostrich armor 'Platemail' to 'Plate'"
- "Mark BendingStyles#get(String) as nullable"

These messages make it easy to tell what's going on.

Sometimes, if the reason for the change is not immediately obvious, additional explanation is needed. This should be added in the description part of the commit message. The description should be added after two line breaks of the first line.

A good example of a commit description is:

.. code-block::
   
   Fix duplicate scanning of same BlockPos in FloodFill
   
   BlockPos was only added to processedBlocks after processed,
   which meant pos added to queue were still added
   again

From reading the commit message, it might not be immediately obvious why there was a duplicate scanning issue, which was cleared up by the commit description.

Branch naming
-------------

The purpose of a branch name is to convey the feature, hotfix, or version in question in a concise and standardized manner.

For version branches, the branch name should simply be the version name (e.g., "a5.0"). For other branches, name the branch after the feature or fix in question. Words in branch names should be lowercase and separated with hypens. Examples:

- a5.0 (for the version branch for alpha 5.0)
- config-balancing (branch for adjusting default configuration values)
- fix-water-arc-crash (branch for crash hotfix; branches don't need to explain things in detail)
