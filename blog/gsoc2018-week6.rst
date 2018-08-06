GSoC with git, week 6: do you like debugging stories?
=====================================================

:date: 2018-06-11 16:13
:slug: gsoc2018-week-6
:authors: Alban
:lang: en
:summary: Well, I have one.
:category: GSoC 2018
:tags: dev, git
:status: published

Three “big” things happened last week:

 - I re-sent my patch converting the TODO help functionnality to the
   list twice; `the first time`__ to remove the changes concerning the
   newlines, `the second time`__ to rename a variable.

 - I rewrote the ``edit-todo`` functionnality to C, which `I just sent
   to the list`__.

 - I began to rewrite the reflog manipulation part in C. About that…

__ https://public-inbox.org/git/20180605125334.14082-1-alban.gruin@gmail.com/
__ https://public-inbox.org/git/20180607103012.22981-1-alban.gruin@gmail.com/
__ https://public-inbox.org/git/20180611135714.29378-1-alban.gruin@gmail.com/

My first git debugging story
----------------------------

git has an extensive test suite. It covers a lot of cases, and when I
make a mistake while rewriting code, there is at least one test that
fails. It’s a very valuable tool, in short, and it’s very easy to use.

.. code-block:: shell

   $ make test

This command is enough to run the entire test suite. All the scripts
are in the ``t/`` folder of the source, and are in separate shell
scripts, which can be run independently. The most important test for
interactive rebase is ``t3404-rebase-interactive.sh``.

Onto the reflog part of ``rebase -i``. The first thing you should know
is that ``git-rebase.sh`` defines a ``output()`` function, which
silences commands, except when they fail. This is very important.

One of the first things done by interactive rebase is to call
``setup_reflog_action()``. In turn, it will change the reflog message
from ``rebase`` to ``rebase -i (start)`` by calling
``comment_for_reflog()`` (the value is stored in the environment
variable ``$GIT_REFLOG_ACTION``), and then it checkouts the commit on
which the rebase takes place. The checkout operation is silenced with
``output()``.

Later in the process, another function, ``checkout_onto()``, uses
``$GIT_REFLOG_ACTION``, without changing the message. This function
does a silent checkout, too.

I started by naively rewriting ``setup_reflog_action()``, calling
``git-checkout`` with the proper environment variable, setting
``$GIT_REFLOG_ACTION``, and then exiting. And here, the 75th test of
``t3404``, “rebase -i produces readable reflog”, broke. The first
reflog was not ``rebase -i (start)`` anymore, but only ``rebase``. But
why? I set the environment variables correctly…

So I tried to debug ``git-checkout``. gdb is very tricky to use here,
as ``git-checkout`` is called by ``git-rebase--helper``, itself called
by ``git-rebase.sh``, itself called by ``git``. Back to the good ol’
``printf`` debugging technique.

In ``checkout.c``, only one function uses the reflog message:
``update_refs_for_switch()``. So, I added some ``printf()`` to know
which codepath was taken, and what was the reflog message. Basically,
I had that:

.. code-block:: c
    :hl_lines:

	reflog_msg = getenv("GIT_REFLOG_ACTION");
	if (!reflog_msg)
		strbuf_addf(&msg, "checkout: moving from %s to %s",
			old_desc ? old_desc : "(invalid)",
			new_branch_info->name);
	else
		strbuf_insert(&msg, 0, reflog_msg, strlen(reflog_msg));

	fprintf(stderr, "%s\n", buf.buf);
	if (!strcmp(new_branch_info->name, "HEAD") &&
	    !new_branch_info->path && !opts->force_detach) {
		/* Nothing to do. */
		fputs("1\n", stderr);
	} else if (opts->force_detach || !new_branch_info->path) {
		/* No longer on any branch. */
		fputs("2\n", stderr);
		/* … */
	} else if (new_branch_info->path) {
		/* Switch branches. */
		fputs("3\n", stderr);
		/* … */
	}

Guess what was the value of ``reflog_msg``. ``rebase -i (start)``? Of
course. The log also showed that the 3rd code path was taken, but the
reflog message is set in the second one… And there was only *one*
output from a checkout. I was clearly missing something.

I tried another approach: in the main function of
``git-rebase--interactive.sh``, I manually set ``$GIT_REFLOG_ACTION``
to ``r``. And that ``r`` appeared in the reflog. And then I
understood. It was the checkout from ``checkout_onto()`` that set the
value of the reflog, not the one from ``setup_reflog_action()``! And I
did not see its output because it was silenced by ``output()``. How
ironic. I also forgot that a process can’t change its parent’s
environment variables. The fix was simply to call
``comment_for_reflog()`` at the beginning of ``checkout_onto()``.

One frustrating hour of debugging for a one-line fix, I’m not really a
fan. Especially when that fix goes away in the next commit!

`On IRC`__, ``oiaohm`` said `USDT probes`_ could be useful, and
``dscho`` said he puts ``getpid()`` in his debug messages for that
reason. USDT probes seems interesting, I hope to talk about them in a
future post.

__ http://colabti.org/irclogger/irclogger_log/git-devel?date=2018-06-09#l58
.. _USDT probes: https://lwn.net/Articles/753601/
