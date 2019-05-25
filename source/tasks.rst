===============
Task Management
===============

bV5 currently uses a cooperative scheduler to run tasks. This means
you have to **cooperate** with the code. If you never call
:c:func:`delay`, the system will never get a chance to think, and
crash, so make sure you call it!

.. c:function:: Mutex new_mutex()

    Create a new mutex that can be locked and unlocked.

.. c:function:: void take_mutex(Mutex mutex)

    Lock a mutex. This will prevent other tasks that also want to
    lock it from running until you release it (or the contray, if
    it's already locked).

    :param mutex: The mutex that should be locked.

.. c:function:: void release_mutex(Mutex mutex)

    Unlock a mutex. This allows other tasks to lock the mutex again.

    :param mutex: The mutex that should be unlocked.

.. c:function:: tid_t start_new_task(void (*taskf)(void), uint8_t priority)

    Create a new task, and start it. It's best explained with an example:

    .. code-block:: c

        void my_task() {
            while (1) {
                delay(10);  // Do nothing!
            }
        }

        tid_t my_task = start_new_task(&my_task, 5);

    The slightly odd name ``tid_t`` is just short for "Task ID Type".

    :param taskf: A pointer to a task function.
    :param priority: The task priority. This should probably be
                     between 4 and 10. Lower priority gets precedence.


.. c:function:: void swap_task()

    Releases execution to the scheduler. This allows another taks to
    run. This is essentially the same as using ``delay(0);``

.. c:function:: void stop_current_task()

    Kills the current task. This function will never return.

.. c:function:: void stop_task(tid_t task_id)

    Stops and cleans up given task.

    :param task_id: The :c:data:`tid_t` returned from
                    :c:func:`start_new_task`
