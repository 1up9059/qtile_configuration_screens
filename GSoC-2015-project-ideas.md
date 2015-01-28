WIP: See https://wiki.python.org/moin/SummerOfCode/OrgIdeasPageTemplate

##### Table of Contents

* [About Qtile](#about-qtile)
* [Getting Started](#getting-started)
* [Writing Your Application](#writing-your-application)
* [Project Ideas](#project-ideas)

# About Qtile

Qtile is a full-featured, hackable tiling window manager written and configured
in Python.  As a tiling window manager, Qtile is designed to be very
lightweight; however, it has a very powerful configuration and scripting
interface.  The look and feel of Qtile is set with a Python config file, and
Qtile ships with a base set of widgets and layouts that can be set-up, but users
are also able to add functionality to existing widgets or create their own
widgets and layouts.  In addition, Qtile exposes all of the functionality of
the window manager through a scripting interface, such that events normally run
on the keyboard or with the mouse can be triggered through scripts.  This also
alows for unit testing of Qtile, that it properly displays windows, and
layouts, that they properly move and position windows.

todo

Mailing List: http://groups.google.com/group/qtile-dev

IRC: irc://irc.oftc.net:6667/qtile

# Getting started

todo

# Writing your application

todo

# Project Ideas

## An asyncio-based DBus library.

This is not really qtile specific, but will be needed by the general python
community going forward.  Having something like this would also get rid of our
hack to work around not having it.

Related Links:

Skills:

Difficulty:

## Experimental Wayland support

Wayland is the way forward, so it would be good to start working on
experimental support for it to see just how much of qtile is re-usable. The
real value of qtile is in the user contributed widgets, so we'd like to
preserve that code going forward

Related Links:

Skills:

Difficulty:

## Better layout serialization

Qtile currently doesn't serialize layout state across restarts, and it would be
nice to have that. It may be possible to just pickle and pass the state of each
layout, but this would probably also involve a re-design of the layout code to
share some implementation pieces of the window shape(s).

Related Links:

Skills:

Difficulty: