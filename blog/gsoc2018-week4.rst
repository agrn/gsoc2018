GSoC with git, week 4
=====================

:date: 2018-05-26 18:29
:slug: gsoc2018-week-4
:authors: Alban
:lang: en
:summary: This week, I attempted to submit my work to the mailing
          list.
:category: GSoC 2018
:tags: dev, git
:status: published

`Last week`_, I started to move the ``preserve-merges`` functionnality
to a new script.

I detailed a little bit why I wanted to do so in the first post, so
basically, it boils down to two reasons:

 - less code in the script: I have a clear view of what I have to
   convert, and
 - ``preserve-merges`` is deprecated by ``rebase-merges``.

This patch series is done in four commits:

 1. duplicate ``git-rebase--interactive.sh`` to
    ``git-rebase--preserve-merges.sh``
 2. remove non-merge-preserving code from
    ``git-rebase--preserve-merges.sh``
 3. make ``git-rebase.sh`` call ``git-rebase--preserve-merges.sh`` when
    it is called with ``-p``
 4. remove merge-preserving code from ``git-rebase--interactive.sh``

First, I made my changes on the ``master`` branch of git. Johannes,
who made ``git rebase -i --root`` to use more from the sequencer (see
his branch, |js/sequencer-and-root-commits|_), advised me to rebase my
branch onto his, as it would allow me to remove even more code from
``--interactive.sh``.

After that, I sent my work to the mailing list.

`1st attempt`_
--------------

The first commit of my patch series consist to duplicate
``git-rebase--interactive.sh``. Nothing more, nothing less.

But when I rebased my work onto Johannes’, I forgot to update
``git-rebase--preserve-merges.sh``, and I ended up with two different
files, as pointed out by Stefan Beller. So I fixed it and sent a new
version of my patch series.

`2nd attempt`_
--------------

Nobody said anything about the second attempt, but I noticed that I
forgot to remove a function and that a bunch of variables could be
stripped from ``git-rebase--interactive.sh``.

So I made a third version of my patch series.

`3rd attempt`_
--------------

Johannes made some comments about the commit messages in my patch
series. Basically, they did not elaborate enough on what they did. I
changed them following his advice (`here is the branch`_), but I did
not re-submitted the series to the mailing list yet. Stefan was pretty
enthusiastic about its state, though.

The current iteration of the patch takes
``git-rebase--interactive.sh`` from 1070 lines down to 284 lines. `See
it by yourself`_!

Feel free to check out the blogs of `Pratik Karki`_ and
`Paul-Sebastian Ungureanu`_, my co-students participating to the GSoC
with git!

.. |js/sequencer-and-root-commits| replace:: ``js/sequencer-and-root-commits``

.. _Last week: {filename}gsoc2018-week3.rst
.. _js/sequencer-and-root-commits:
    https://github.com/gitster/git/commits/js/sequencer-and-root-commits

.. _1st attempt:
    https://public-inbox.org/git/20180522133110.32723-1-alban.gruin@gmail.com/

.. _2nd attempt:
    https://public-inbox.org/git/20180522211625.23893-1-alban.gruin@gmail.com/

.. _3rd attempt:
    https://public-inbox.org/git/20180524114958.26521-1-alban.gruin@gmail.com/

.. _here is the branch:
    https://github.com/agrn/git/commits/ag/move-rebase-p-patch-v4

.. _See it by yourself:
    https://github.com/agrn/git/commit/b3dc16f7382deb4e3a8062f35bdc1b60ec1855cc

.. _Pratik Karki:
    https://prertik.github.io/

.. _Paul-Sebastian Ungureanu:
    https://ungps.github.io/
