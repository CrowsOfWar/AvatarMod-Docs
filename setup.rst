Setup
=====

This document shows you how to set up the AvatarMod development environment so you can start working on it.

Prerequisites
-------------

You need `Git <https://www.atlassian.com/git/tutorials/what-is-version-control>`_ to work on avatar mod. You should have at least `basic experience <https://www.atlassian.com/git/tutorials/saving-changes>`_ with Git before trying to develop AvatarMod.

Please have also good experience with Java and Forge before developing AvatarMod.

You must know how to open a terminal in this tutorial. Since you probably already know how to, and this varies by the operating system, this is not explained here.

Setting up
----------

Clone the GitHub repository onto your computer using this command. It will create a folder called AvatarMod.

.. code-block:: sh
   
   git clone https://github.com/ProjectKorra/AvatarMod.git

The next instructions are specific to your IDE:

- `Eclipse <#eclipse>`_
- `IntelliJ <#intellij>`_

Eclipse
-------

If you are running Eclipse, run this command:

.. code-block:: sh
   
   ./gradlew setupDecompWorkspace eclipse

Point Eclipse to the eclipse folder, and the MDKMinecraft project will show up on the package explorer. Eclipse has been successfully set up!

IntelliJ
--------

If you are running IntelliJ, you don't need to use the command line any more and can use IntelliJ's built-in Gradle integration. Start by importing the Gradle project. If you are in the Welcome screen, choose :code:`Import Project`; if you have a window open, choose :code:`File > New > Project From Existing Resources`.

Navigate to the directory that you cloned the AvatarMod repo into, and then click on :code:`build.gradle`.

If the dialog says "Gradle location not specified", check "Use gradle wrapper task configuration". Then click OK. This may take a while, but the project will open.

When the project opens, in the Gradle projects view, find the tasks :code:`setupDecompWorkspace` and :code:`idea`. Run them both, and you should be good to go.

Run Configuration
-----------------

The last thing you need to do is configure your Run Configurations.

Sometimes run configurations aren't added, so just create them manually if that is the case. Make two configurations, one for the Client (what you use to play minecraft) and Server (the command line server without graphical capabilities). The client main class is :code:`GradleStart`, and the server main class is :code:`GradleStartServer`.

Finally, even if run configurations were automatically set up, you should add username to the run configurations. In the Program arguments section, add the following: :code:`--username <YourName>`. This will ensure Minecraft will start with your username every time. If you don't do this, Minecraft will assign a different name every time so your AV2 progress will be reset each time you restart Minecraft.
