GSoC with git, week 10
======================

:date: 2018-07-10 18:19
:slug: gsoc2018-week-10
:authors: Alban
:lang: en
:summary: I converted the first initialisation part this week.
:category: GSoC 2018
:tags: dev, git
:status: published

I wanted to convert the rest of ``git-rebase--interactive.sh`` last
week, but I have not been able to, unfortunately.

Anyway, I rewrote ``init_revisions_and_shortrevisions()``, and fixed
some points about my last patch series.  Remember, I had to merge my
smalls series into a big one.  To avoid spamming the list, I decided
to send those fixes along with the converted version of
``complete_action()`` and ``init_revisions_and_shortrevisions()``.
You can find the series here__.

Right now, I’m preparing to rewrite ``write_basic_state()``, and then
``init_basic_state()``.  And then, I can finally turn interactive
rebase into a builtin.

But it won’t be the end of the story!  I will have plenty of cleanup
to do, like moving functions from ``sequencer.c`` to
``rebase-interactive.c``, or avoid useless operations on files.  And
that could keep me busy a bit after the GSoC.

__ https://public-inbox.org/git/20180710121557.6698-1-alban.gruin@gmail.com/
