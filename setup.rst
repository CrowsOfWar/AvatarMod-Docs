Setup
=====

This document shows you how to set up the AvatarMod development environment so you can start working on it.

Prerequisites
-------------

You need Git to work on avatar mod. You should use git from the command line when installing, as that is

Please have experience with Java and Git before trying to work on AvatarMod. A good knowledge of Forge is also recommended.

Setting up
----------

Clone the GitHub repository onto your computer using this command. It will create a folder called AvatarMod. You can pass a second argument to the command to change the location (or :code:`.` to specify the current folder).

.. code-block:: sh
   git clone https://github.com/CrowsOfWar/AvatarMod.git

The next instructions are specific to your IDE:

- `Eclipse <#eclipse>`_
- `IntelliJ <#intellij>`_

Eclipse
-------

If you are running Eclipse, run this commands:

.. code-block:: sh
   ./gradlew setupDecompWorkspace eclipse

Point Eclipse to the eclipse folder, and the MDKMinecraft project will show up on the package explorer. Eclipse has been successfully set up!

IntelliJ
--------

If you are running IntelliJ, you don't need to use the command line any more and can use IntelliJ's built-in Gradle integration. Start by importing the Gradle project. If you are in the Welcome screen, choose :code:`Import Project`; if you have a window open, choose :code:`File > New > Project From Existing Resources`.

Navigate to the directory that you cloned the AvatarMod repo into, and then click on :code:`build.gradle`.

If the dialog says "Gradle location not specified", check "Use gradle wrapper task configuration". Then click OK. This may take a while, but the project will open.

When the project opens, in the Gradle projects view, find the tasks :code:`setupDecompWorkspace` and :code:`genIntellijRuns`. Run them both, and you should be good to go.

Run Configuration
-----------------

The last thing you may want to do is edit your run configurations as follows:

- Add the program argument :code:`--username <username>`, which will make the development environment use a specific username every time you run Minecraft.
- Change the location of your
