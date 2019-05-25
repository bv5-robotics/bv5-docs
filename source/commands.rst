====================
The bV5 Command Line
====================

bV5 makes use of a handy handy command line program called... `bv5`!
It manages all the interactions with the brain so you don't have to.
It also has project management and updating.. maybe. I've not really
tested that part enough, so I'm not going to suggest using it and
expecting it to work :P. It's under ``bv5 packages`` if you want to
have a peek though.

Installing
----------

.. code-block:: c

    // TODO: This. Maybe.

Right now the best solution is to ``git clone <the repo>`` then use
``pip install --user -e .`` to get setup. A proper solution will
probably come later.

Updating Firmare
----------------

The ``bv5 dfu`` command will open a firmware updater for you to use
to, you guessed it, update firmware. It doesn't do anything fancy,
and doesn't have any bells and whistles, but it's a darn sight
smaller than the official one so is a nice way to save installing a
massive firmware updater from VEX.

Building Your Project
---------------------

By way of a ``Makefile``, you can just run the command ``make``. If
you are on Windows, that command probably doesn't exist. Using
``bv5 make``, it will search for a copy of ``make``, then if it fails
to find it, it will attempt to use the Windows Subsystem for Linux.

If you need a copy of make, or instructions for how to setup the WSL,
Google is your friend.

bV5 also has a command named ``bv5 make watch``, which will monitor
your project source, and then re-compile it when anything changes. It
can also optionally upload the new binary to the brain. Use
``--help`` for more information on that.

Uploading Your Project
----------------------

So you've compiled your program, ey? ``bv5 upload``! As above, use
``bv5 upload --help`` for more information about the options you can
pass to this command.

Cool V5 Utilities
-----------------

There's a lot of cool stuff in ``bv5 v5``. Here are some of them:

 * ``erase``: Clear your brain
 * ``name [robot name]``: Set or get the robot's name
 * ``team [team name]``: Set or get the robot's team
 * ``tree``: List all the files on the robot.
 * ``speedtest``: Test the connection speed. This is useful for
   wireless uploading (when you plug into the controller instead of
   the brain)
 * ``term``: Open a serial terminal to the brain. You can use
   :c:func:`printf` and :c:func:`getchar` to communicate from the
   brain.
 * ``set <variable> <value>``/``get <variable>``: Modify system
   variables. These are general purpose storage spaces on the brain.
   Fun fact: They persist even after a factory reset of the brain!
 * ``read <file>``: Download a file off the brain. It's like
   ``upload``, but cooler!
 * ``last``: Ever wondered when the last time you used bV5 on a robot
   brain was? Of course not, but hey, there's a command for it.

``bv5 v5`` is where I usually put random things, so it's worth
running ``bv5 v5 --help`` if you get bored, to see if I've added
anything fun :).
