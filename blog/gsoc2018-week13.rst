GSoC with git, week 13: leftovers
=================================

:date: 2018-08-02 16:07
:slug: gsoc2018-week13
:author: Alban
:lang: en
:summary: The conversion is done, but there is still some work to do
:category: GSoC 2018
:tags: dev, git
:status: published

The conversion of ``git-rebase--interactive.sh`` is done, but I still
have some things on my todo list.

Improving the todo list processing
----------------------------------

If you look at |complete_action()|__, you may notice that a lot of
functions read the todo list from the filesystem, parses and processes
it, then write it back.  It’s a remnant of the shell script:
``git-rebase--interactive.sh`` had to call those functions, but there
was no way to pass around the parsed representation of the todo list.
This is no longer a problem since this is rewritten in C: we can
generate a todo_list once, pass it to every function that need it, and
write it once to the disk.

However, some of these functions are still needed by
``git-rebase--preserve-merges.sh``, so, instead of dropping the
read/parse/process/parse dance, we need to move the “process” part to
its own function.  This way, we can use the “process” function from
``sequencer.c``, while still being able to use it from ``rebase -p``.

__ https://github.com/agrn/git/blob/fc0bb07d835cebb1ce03b370d29676065f093c01/sequencer.c#L4561

Warn the user when a commit is dropped with ``--edit-todo``
-----------------------------------------------------------

By default, git does not warn if you drop a commit from the
todo-list after the initial edit.  `This behaviour can be changed`__: you
can be warned, or postpone the rebase if this happens.

But this parameter won’t be taken into account when you edit the todo
list later with ``--edit-todo``.  This is because ``edit_todo()`` and
``complete_action()`` are completely different codepathes (and
``edit_todo()`` does **not** check if the new todo list is correct!),
so git’s behaviour is very inconsistent here.

`This change was proposed by Phillip Wood`__.

__ https://git-scm.com/docs/git-rebase#git-rebase-rebasemissingCommitsCheck
__ https://public-inbox.org/git/3bfd3470-4482-fe6a-2cd9-08311a0bbaac@talktalk.net/

Clean ``sequencer.c``
---------------------

Some functions from the sequencer are only needed by interactive
rebase, so it would be nice to move them to ``rebase-interactive.c``
to unclutter the sequencer.

Right now, I’m working on improving the todo list processing.  All of
this will keep me busy for a little while after the GSoC.

.. |complete_action()| replace:: ``complete_action()``
