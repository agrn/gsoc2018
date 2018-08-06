GSoC with git, week 12: finally a builtin
=========================================

:date: 2018-07-24 20:52
:slug: gsoc2018-week12
:authors: Alban
:lang: en
:summary: The main conversion is done, at last
:category: GSoC 2018
:tags: dev, git
:status: published

So, last week, I rewrote interactive rebase as a builtin.  This task
was mostly trivial.  Mostly.

When I tried to call ``complete_action()`` after
``sequencer_make_script()``, some tests related to submodules in t3404
broke.  Johannes Schindelin found out that the issue is related to a
cache that was not properly refreshed.  Many thanks to him.

There also was a memory bug in the code.  In short, I passed a string
acquired from the command line to a function that tried to ``free()``
it later, causing the program to segfault.  A ``xstrdup_or_null()``
fixed the issue.

Anyway, `I sent the result to the mailing list`__, and I am waiting
for feedback.

__ https://public-inbox.org/git/20180724163221.15201-1-alban.gruin@gmail.com/
