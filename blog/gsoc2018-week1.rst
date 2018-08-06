GSoC with git, week 1 : community bonding
=========================================

:date: 2018-05-05 00:07
:slug: gsoc2018-week-1
:authors: Alban
:lang: en
:summary: This summer, I’m working on git. This week, it’s the
	  community bonding.
:category: GSoC 2018
:tags: dev, git
:status: published

For the GSoC 2018, I proposed to rewrite ``git-rebase--interactive``,
or interactive rebase for short. You may not know this specific
feature of git_, but it’s very useful. Basically, it allows you to
reorganize commits in an interactive way. If you make a lot of commits
like “quick fix”, followed by a “quick fix 2”, you should `definitely
check out this feature`_!

Rewriting interactive rebase
----------------------------

But why rewrite it? Well, currently, it’s written as a shell
script. They have some advantages (they’re fast to write), but have a
lot of drawbacks: they’re slow (especially on Windows), not really
reliable, portability is hazardous… The goal would be to rewrite
interactive rebase in C. It’s not perfect portability-wise, but it’s
really better than shell scripts.

While I would like to rewrite everything during the three monthes of
the GSoC, the goal is primarly to write good code that is actually
mergeable, not to rush the project and to produce unusable code.

Community bonding
-----------------

Currently, it’s the community bonding period. During this period, I’m
exchanging with my mentors, Christian Couder and Stefan Beller, others
members of the community (most notably, Johannes Schindelin, the
creator of interactive rebase), and the two others GSoC students,
`Pratik Karki`_ and `Paul-Sebastian Ungureanu`_. They are also
rewriting parts of git, respectively, ``git-rebase`` and
``git-stash``. Check out their own posts, too!

Currently, there is some movements in ``git-rebase``: the deprecation
of ``--preserve-merges`` (or ``-p`` for short) in favour of the new
``--recreate-merges`` [#]_, and a bit of refactoring, too.

Since ``-p`` uses interactive rebase internally, and we don’t really
want to bother with it, I won’t rewrite it. Currently, ``-p`` is in
the same file as interactive rebase. So, I will move ``-p`` in its own
file, and then let it bitrot. For that, I will take advantage of `Wink
Saville’s patches`_, which refactored ``-p``-specific code into their
own functions.

So, this is the beginning of three exciting monthes, and of a weekly
blog series about this journey.

.. _git: https://git-scm.com/

.. _definitely check out this feature:
    https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History#_changing_multiple

.. _Pratik Karki:
    https://prertik.github.io/post/git-an-unexpected-journey/

.. _Paul-Sebastian Ungureanu:
    https://ungps.github.io/2018/05/03/week-1-(introduction)/

.. _Wink Saville’s patches:
    https://public-inbox.org/git/cover.1521839546.git.wink@saville.com/

.. [#] Now called ``--rebase-merges``
