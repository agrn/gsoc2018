GSoC with git, week 7
=====================

:date: 2018-06-18 19:30
:slug: gsoc2018-week-7
:authors: Alban
:lang: en
:summary: Last week, I worked on my series about the reflog, and some
          of my patches were accepted in ``pu``.
:category: GSoC 2018
:tags: dev, git
:status: published

This week, I don’t have a long story to tell, only a few things.

Most of my work was on polishing my patches about the reflog
manipulation I mentionned `last week`_.  I sent the patches to the
list `this afternoon`__, and I already received some
comments. Basically, I have to correct some details in the commit
messages, change some names, and stop using ``die()`` in the
sequencer. As it’s part of libgit, dying without giving callers a
chance to clean up after themselves is not really a good practice.

I started to work on converting the bulk of ``complete_action()``,
where most of the code of ``git-rebase--interactive.sh`` is remaining.

My patches about splitting ``rebase -i`` and ``rebase -p`` was
graduated to ``next``, and the two series converting
``append-todo-help()`` and the “edit todo” functionnality were
accepted in pu!

Also, last week, there was the first evaluation of the program, which
I passed successfully!

__ https://public-inbox.org/git/20180618131844.13408-1-alban.gruin@gmail.com/
.. _last week: {filename}gsoc2018-week6.rst
