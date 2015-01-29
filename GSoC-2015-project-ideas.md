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

Here are some potential ideas we have thought may be appropriate, however, feel
free to propose an idea to us that you are interested in.

## An asyncio-based DBus library.

As a part of the 0.9.0 release, we [dropped all C extensions as hard
dependencies](/blob/develop/CHANGELOG#L3), including dropping the PyGTK/GLib
event loop in favor of the new Python asyncio event loop.  However, several of
the widgets, such as the MPRIS widget, rely on D-Bus, and the [python-dbus
library](http://dbus.freedesktop.org/doc/dbus-python/) is [hard
coded](http://cgit.freedesktop.org/dbus/dbus-python/tree/dbus/mainloop/glib.py)
to use the gobject event loop.  We have come up with a
[hack](https://github.com/qtile/qtile/commit/82256a47b9f954b2bb9922b821dfd6d529dc5437)
to get around this limitation, however, it is not ideal.

The point of this project is to implement a D-Bus library that can work
directly with asyncio (and preferably other future event loops).  In other
words, this would be writing a pure Python D-Bus binding that can be configured
to fire off asyncio calls.  Tools such as cffi could be used to make the
necessary libdbus calls.

This project is not really Qtile specific, but will be needed by the general
python community going forward if the asyncio event loop is going take hold.

Skills:

Difficulty:

### Related Links

* [Python asycio library](https://docs.python.org/3/library/asyncio.html)
* [gbulb](https://bitbucket.org/a_ba/gbulb), a Python 3.3+ library that wraps
  the GLib event loop in asyncio (but that we have not been able to get working
  with qtile)
* Current [python-dbus](http://dbus.freedesktop.org/doc/dbus-python/) C extension

## Experimental Wayland support

Wayland is the way forward, so it would be good to start working on
experimental support for it to see just how much of qtile is re-usable. The
real value of qtile is in the user contributed widgets, so we'd like to
preserve that code going forward

**Skills:**

**Difficulty:**

### Related Links

## Better layout serialization

Qtile currently doesn't serialize layout state across restarts, and it would be
nice to have that. It may be possible to just pickle and pass the state of each
layout, but this would probably also involve a re-design of the layout code to
share some implementation pieces of the window shape(s).

**Skills:**

**Difficulty:**

### Related Links