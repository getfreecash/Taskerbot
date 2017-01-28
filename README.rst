MoviesMod (a variation on /r/Android's Taskerbot)
=========

A reddit bot with two primary functions:

* to remove a post and inform users of the removal reason at the same time (very useful for removing posts with thousands of upvotes that make it to #1 or /r/all)
* to ban users

Both of these can be done directly from the comments using simple text commands!

**You must restart MoviesMod if you add or remove a subreddit moderator.** MoviesMod loads the subreddit's list of moderators and removal reasons at startup. To refresh these, send MoviesMod a message containing ``@refresh Subreddit`` (e.g. ``@refresh movies`` to reload ``/r/movies``'s configuration). Note that this is case sensitive, so make sure it's the same as what's in the configuration file (see `Configuration file`_).

Usage
=====

MoviesMod can be invoked by any comment containing one of
the following commands (**all arguments are required**):

- ``@rule {reason} [note]``: **removes a post, leaving an appropriate flair
  and comment**. ``reason`` is one of the removal reasons' keys (see `Removal
  reasons`_). If no corresponding removal reason is found, the ``Generic``
  reason is used instead. If ``note`` is provided, it will be added to the
  message. **This command also removes comments but it doesn't reply to the comment with the removal reason, so it's not very useful in that regard.**

  Example: ``@rule spam``, ``@rule repost this is a note``.

- ``@ban {duration} "{reason}" "{message}"``: **bans the comment/thread's
  author**. All three arguments are required. ``duration`` is the ban's
  duration in days, ``reason`` is the ban's reason (visible only to mods), and
  ``message`` is the message the banned user will receive. ``reason`` and
  ``message`` must be between quotations (``""```) (escapes are not supported).

  Example: To ban a spammer for 3600 days for spamming somedomain.com, reply to their post or comment with ``@ban 3600 "spammer" "repeatedly spamming somedomain.com"``

Multiple actions can be specified at once. If MoviesMod was invoked by a
comment, it will automatically remove it - which means that the OP of the removed post or the banned user will not know which moderator removed their post/banned them.

Only moderator reports and comments made by/mails sent by the subreddit's
moderators are checked.

PS. Reddit silently ignores reports on removed posts, so MoviesMod won't see
those.

Setup
=====

MoviesMod requires Python 3. For a list of required Python 3 libraries, see
``requirements.txt``. Install ``pip`` using ``easy_install``.

You'll first want to create a new account and make it a mod of your subreddit.
Required permissions are: access, flair, posts, and wiki.

Configuration file
~~~~~~~~~~~~~~~~~~

The ``config.yaml`` file must contain the bot's username, password and a list
of subreddits to patrol. The syntax is as follows:

.. code:: yaml

   User Agent: 'user agent here'
   Client ID: 'client id here'
   Client Secret: 'client secret here'
   Username: 'bot username here'
   Password: 'bot password here'
   Subreddits:
       - 'SomeSubreddit'
       - 'AnotherSubreddit'
       - 'YetAnotherSub'

To obtain the client ID and secret, please follow reddit's `OAuth2 First Steps
guide`_.

Use a `YAML validator`_ to make sure the configuration file is valid.

.. _Removal reasons:

Removal reasons
~~~~~~~~~~~~~~~

This step must be performed for every new subreddit you want MoviesMod to
patrol.

1. Create a new wiki page on the subreddit called ``moviesmod``.
   For example, if you want MoviesMod to patrol ``/r/movies``, you must create
   ``/r/movies/wiki/moviesmod``.
2. Fill the page with the reasons you want -- ``Header``, ``Footer`` and
   ``Generic`` are required reasons so make sure you include at least those: https://www.reddit.com/r/MovieMods/wiki/moviesmod
   
   **Reasons' keys cannot contain spaces** (e.g. in the example above, ``1``
   and ``spam`` are fine, but ``reason 2`` is not).

   Each custom removal reason must have two entries: ``Flair``, which will be
   what the removed thread's flair is set to, and ``Message``, which is the
   comment MoviesMod will leave in the thread.

   Also note that MoviesMod will automatically replace all instances of
   ``{author}`` in the ``Header`` and ``Footer`` with the author's username.

   You can check `/r/Android's taskerbot wiki page`__ for a real example (click
   "View source" in the bottom right).

   __ https://www.reddit.com/r/Android/wiki/taskerbot
3. Create a new wiki page on the subreddit called ``moviesmod_logs``. You can
   keep it blank (MoviesMod will always append to it). This page will be used
   to log mod actions.

PS. Make sure to make the wiki pages editable by mods only.

Use a `YAML validator`_ to make sure the configuration file is valid.

.. _YAML validator: http://www.yamllint.com/
.. _YAML: http://www.yaml.org/
.. _OAuth2 First Steps guide: https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example#first-steps
