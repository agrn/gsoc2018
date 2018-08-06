GSoC with git, week 11
======================

:date: 2018-07-17 20:40
:slug: gsoc2018-week11
:authors: Alban
:lang: en
:summary: More conversion of the initialisation part this week.
:category: GSoC 2018
:tags: dev, git
:status: published

Last week, I rewrote ``write_basic_state()`` and
``init_basic_state()`` in C.

``write_basic_state()`` is called when you start a rebase.  It’s very
trivial: it writes a lot of informations about the current rebase in
the ``.git/rebase-merge`` directory (one variable by file), so you
don’t have to write the same parameters everytime you do a ``git
rebase --continue``.

Such a conversion is trivial, too, when you have ``write_file()``.
Its name is pretty explicit ;)

``init_basic_state()`` is even shorter: it creates the state
directory, create a file called ``interactive``, and then calls
``write_basic_state()``.

This conversion is done now, there is only the main function
remaining.  The next step is to convert it to a builtin.
