===============
Getting Started
===============

Getting Hold of bV5
-------------------

Right now there are no public repositories of binaries of bV5. If you
found this site, either you found a random site, or it was linked to
you. In the former case, welcome, stranger. In the, more likely,
latter, there should have been a template archive provided to you in
the same place. Take that archive, and un-archive it. And now you're
done. 🎉.

Introduction to bV5
-------------------

bV5 is a fun little library I wrote for V5. Why? Because I wanted to
have some fun, and I figured I might as well do something productive.

When you open your first project, you have a few files. The ones
you're going to be interested in initially are ``include/main.h`` and
the ones in ``src/``.

Here's a sample from ``user.c``:

.. code-block:: c

    #include "main.h"

    void driver() {
        while (1) {
            /* [ SNIP  */

            /* Make sure to call delay so we can do other stuff */
            delay(20);
        }
    }

If we have a look around, as well as ``driver``, there are ``auton``,
``setup``, ``init``, and ``disabled``.

If you've been following along, you've probably noticed the all have
a little comment saying what they do. Here's what they do again,
anyway:

 * ``driver``: This is the code that is run when you are in driver
   control mode.
 * ``auton``: And this is the code for when you're in autonomous
   mode.
 * ``disabled``: This code is run when the robot is disabled.
 * ``setup``: This function is called before entering any of the
   previous three functions.
 * ``init``: This function is called once when the program starts.
   This is a good place to do things like startup background tasks,
   initialize sensors like ultrasonics, etc.

It's important to note that any time you have a loop that will run
for a long (or infinite) time, you will need to call :c:func:`delay`
somewhere so that the rest of the system can tick along nicely.

``main.h`` is a globally included header file. This is a nice place
to define things you want to be global.

Where Next?
-----------

Anywhere, really. If you want to dive right in, the API reference is
just for you, if you want to learn more about how to use the command
line, that section'll help, and if you're looking to run some extra
tasks or want to know a little about them, then have a look at Task
Management.

This is the part where I'd usually refer you over to the examples,
but they're somewhat lacking because I've just spend 4 hours writing
all these pages. I don't really feel like making any examples. When I
do, I'll replace this.
