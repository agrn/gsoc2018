GSoC with git, week 2
=====================

:date: 2018-05-12 20:04
:slug: gsoc2018-week-2
:authors: Alban
:lang: en
:summary: It’s still the community bonding, and this week was pretty
          calm.
:category: GSoC 2018
:tags: dev, git
:status: published

So, this week was pretty calm.

Early this week, we talked a bit about git-rebase and how it can be
improved with Christian Couder and Pratik Karki on IRC [#]_. We
exchanged articles and opinions about ``git-rebase``.

We talked a bit about rebase_ with_ history_ and |git-imerge|_, as
described by Michael Haggerty on his blog. His articles are really
interesting (if you have not read them, do it now!) and talk clearly
about the current shortcomings of the current ``rebase``
implementation, but it’s also clearly out of scope of Pratik’s and my
project.

Pratik also shared `an article`_ explaining why ``git-rebase`` should
never be used.

Never say never
---------------

I’ve read such articles in the past, and I’m sure you have already,
too, whether they talk about ORMs, ``goto``, operating systems, the
list goes on.

I don’t really like this kind of articles. Sometimes, they hit the
nail on the head (or at least raise problems), but sometimes, the
author is projecting their use cases and workflow onto you, making
their arguments moot.

The latter applies for this article. Depending on your workflow, you
don’t necessarily have access to the upstream repository, and so to
the final history. For instance, if you’re working on a free software
project where patches must be sent to a mailing list (like git),
rebasing changes instead of merging them remove the noise caused by
merge commits, and allows you to have a clear view of the changes
you’re currently making. When your patches will be merged, the history
of your changes will be linear, no matter how many times you synced up
with upstream. And `as Christian pointed out`_, there actually is a
way to check if every rebased commit is correct without doing it
manually: |--exec|_.

So yeah, whether you should use ``git-rebase`` or not really depends
on your use case and your workflow. If you want a good starting point,
`this article from Atlassian`_ is much more insightful.

Back to GSoC
------------

But aside from ranting, I began to seriously dive into
``git-rebase``’s code, and planning to move ``-p`` to its own script
next week, when the exciting part begins :).

.. |git-imerge| replace:: ``git-imerge``
.. |--exec| replace:: ``--exec``

.. _rebase:
  https://softwareswirl.blogspot.fr/2009/04/truce-in-merge-vs-rebase-war.html

.. _with:
  https://softwareswirl.blogspot.fr/2009/08/upstream-rebase-just-works-if-history.html

.. _history:
  https://softwareswirl.blogspot.fr/2009/08/rebase-with-history-implementation.html

.. _git-imerge:
  https://softwareswirl.blogspot.fr/2013/05/git-imerge-practical-introduction.html

.. _an article:
  https://medium.com/@fredrikmorken/why-you-should-stop-using-git-rebase-5552bee4fed1

.. _as Christian pointed out:
  http://colabti.org/irclogger/irclogger_log/git-devel?date=2018-05-06#l81

.. _--exec:
  https://git-scm.com/docs/git-rebase#git-rebase---execltcmdgt

.. _this article from Atlassian:
  https://www.atlassian.com/git/tutorials/merging-vs-rebasing

.. [#] ``#git-devel`` on irc.freenode.net
