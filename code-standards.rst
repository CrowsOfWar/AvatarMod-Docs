Code standards
==============

Here are some guidelines you must follow to maintain a good codebase.

General tips
------------

* You aren't gonna need it: If you think the system you are designing is overcomplicated, it probably is. Keep a clear idea of what the end goal is, so you won't waste time making an extra-extensible system if it's not going to be used

* Document your code changes here to clearly communicate with other developers. If you want to see what happens when you don't document complicated code, please visit world generation code. Have fun :)

* Remember, you are only going to write the code once (hopefully), but read it many times. So take the extra minute or two to make the code readable, instead of writing as little as possible but creating a confusing mess.

Specific requirements
---------------------

Keep in mind that the Minecraft codebase doesn't always follow the rules, but we will since we are better than the obfuscated, poorly documented, garbled code that Minecraft consists of.

* Avoid one-line `if` statements (without curly brackets). Formatting can get weird

* Avoid switch statements. If-else chains are nicer to read, not much slower to write, and don't require the strange syntax that switch cases consist of (colons and break statements).

* Use tabs for indentation instead of spaces. If you are an avid space user: honestly, at the end of the day, tabs vs spaces doesn't really change your experience. With a proper IDE, you will still be pressing TAB key to indent and using CTRL+arrows to navigate.

* Acronyms should start uppercase but all subsequent letters are lowercase. eg Id or Json, but not ID or JSON.

* Use `@author` annotations. Git blame can also be used, but it's nice to have something explicitly in code

Documentation standards
-----------------------

- Javadoc is for reference, RTD is for explanations

- Assume the reader has knowledge of Java and the Forge API before reading the article

- Explain what each class is for and why it is used

- For player-related classes, explain whether the player can be offline

- Note server and client side availability

- At the end of each class page, include a cookbook section that has a few common or useful examples of code using that class
