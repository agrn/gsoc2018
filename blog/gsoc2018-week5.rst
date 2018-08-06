GSoC with git, week 5: let the conversion begin
===============================================

:date: 2018-06-03 23:50
:slug: gsoc2018-week-5
:authors: Alban
:lang: en
:summary: This week, I made a new attempt to submit my changes to the
	  mailing list, and I started to convert some code.
:category: GSoC 2018
:tags: dev, git
:status: published

As you may know, I made no less than three attempts to upstream my
changes about ``preserve-merges`` `last week`_.

This week began by a `fourth one`_.

4th attempt
-----------

Johannes pointed out some problems with my commit messages on that
series:

 - some lines are too long
 - they do not describe what I wanted to do well.

On the first point, commits messages should wrap at 72 characters, but
I configured Emacs to wrap pretty much anything at 80 characters.

He also wanted to keep the original name of 
``git_rebase__interactive__preserve_merges()`` (which I renamed
``git_rebase__preserve_merges()``), but I decided not to, as this
function is now part of ``git-rebase--preserve-merges.sh``.

Also, Friday, Junio Hamano announced__ that my changes (among many
others) have been merged into the ``pu`` (proposed updates) branch of
git.git. This does not mean that it will necessarily hit ``master``,
but it’s a start.

__ https://public-inbox.org/git/xmqqa7sf7yzs.fsf@gitster-ct.c.googlers.com/

TODO: help
----------

When you start an interactive rebase, you’re greeted with your
``$EDITOR``, containing a list of commits to edit, and an help text
explaining you how this works.

So, this week, I rewrote the code writing this message from shell
to C. You can find the branch here__, and the discussion on the list
here__.

__ https://github.com/agrn/git/tree/ag/append-todo-help-c
__ https://public-inbox.org/git/20180531110130.18839-1-alban.gruin@gmail.com/

The conversion itself is quite trivial, but strings are a rather
curious case here: some of them begins with one or two newlines. As
this becomes useless in C, Stefan advised me to remove them, as this
would probably cause less confusion for the translators, but it
implies changing the translations, as pointed out by Johannes and
Phillip Wood. Right now, no clear opinion has emerged from the
discussion.

Some additionnal work and refactoring could be made once more code has
been converted. For instance, the todo-list file is opened, modified,
and closed several times. Instead, we could create a ``strbuf``, build
it progressively, then write it once to the todo file.

What’s next?
------------

I’ve started to work on converting the ``edit-todo`` functionnality of
interactive rebase. This is also a straightforward change, but it
would also require some future work once the conversion is completed.

.. _last week: {filename}gsoc2018-week4.rst
.. _fourth one:
    https://public-inbox.org/git/20180528123422.6718-1-alban.gruin@gmail.com/
