GSoC with git, week 9: rerolling things
=======================================

:date: 2018-07-02 23:00
:slug: gsoc2018-week-9
:authors: Alban
:lang: en
:summary: I rerolled patches last week
:category: GSoC 2018
:tags: dev, git
:status: published

Last week, I focused onto upstreaming things, and rerolling previous
patches, such as ``append_todo_help()``.

``rebase-interactive.c``
------------------------

As I’m rewriting ``git-rebase--interactive.sh``, ``sequencer.c`` gets
bigger and bigger, with functions that are very specific (like, you
gessed it, ``append_todo_help()``, but also ``edit_todo_help()``).  So
Johannes advised me to create ``rebase-interactive.c`` to hold this
kind of functions.

Johannes pointed out that if ``append_todo_help()`` failed to write to
the todo list, no error message would appear.  I quickly sent a fixup
patch, but I was told that I should not send it separately, but that I
should reroll ``append_todo_help()`` entirely instead.

Also, more naming changes in my reflog series [#]_!  So I ended up
rerolling three patch series last week.  There, Junio said that my
patches should have been part of only one series, and not splitted in
three.  So, he created only one integration branch,
|ag/rebase-i-in-c|__.

I hope to be able to convert the rest of
``git-rebase--interactive.sh`` this week.

.. |ag/rebase-i-in-c| replace:: ``ag/rebase-i-in-c``
__ https://github.com/gitster/git/tree/ag/rebase-i-in-c
.. [#] I keep saying it’s a reflog series, even though it’s not
       really about reflog
