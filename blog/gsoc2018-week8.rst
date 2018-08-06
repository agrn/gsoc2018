GSoC with git, week 8
=====================

:date: 2018-06-25 21:00
:slug: gsoc2018-week-8
:authors: Alban
:lang: en
:summary: Some updates on my series about the reflog manipulation, and
          ``complete_action()``.
:category: GSoC 2018
:tags: dev, git
:status: published

`Last week`_, I sent the first iteration of my patch series about the
reflog manipulation. Today, I sent the `fourth one`__. I had to make
some corrections on comments, commit messages, code style, etc. I will
probably send another iteration soon, because of a naming problem.

I also started to convert ``complete_action()`` last week, and I think
it’s good enough for the list now. I will wait for my work on reflog
to be accepted before sending this series.

I think the next step will be to convert
``initiate_action()``. ``git-rebase--interactive.sh`` starts to be
pretty empty now.

__ https://public-inbox.org/git/20180625134419.18435-1-alban.gruin@gmail.com/
.. _last week: {filename}gsoc2018-week7.rst
