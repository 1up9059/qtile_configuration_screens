WIP: See https://wiki.python.org/moin/SummerOfCode/OrgIdeasPageTemplate

Welcome to Qtile!  In addition to this page, you can also look at the [PSF GSoC
page](https://wiki.python.org/moin/SummerOfCode/2015) for more information.

##### Table of Contents

* [About Qtile](#about-qtile)
    * [Contact Us](#contact-us)
* [Getting Started](#getting-started)
* [Writing Your Application](#writing-your-application)
* [Project Ideas](#project-ideas)
* [Possible Mentors](#possible-mentors)

# About Qtile

Qtile is a full-featured, hackable tiling window manager written and configured
in Python.  As a tiling window manager, Qtile is designed to be very
lightweight; however, it has a very powerful configuration and scripting
interface.  The look and feel of Qtile is set with a Python config file, and
Qtile ships with a base set of widgets and layouts that can be set-up, but users
are also able to add functionality to existing widgets or create their own
widgets and layouts.  In addition, Qtile exposes all of the functionality of
the window manager through a scripting interface, such that events normally run
on the keyboard or with the mouse can be triggered through scripts, made
possible by the powerful introspection features of Python.  This also allows
for unit testing of Qtile, that it properly displays windows, and layouts, that
they properly move and position windows.  By using Python, Qtile is very
extensible and users can easily modify or add functionality as they see fit.

## Contact Us

If you are interested in working with Qtile (whether for GSoC or not), you can
join our mailing list, where we both discuss development issues and provide
help to users.  You can also contact us through IRC, which is where most of our development discussion takes place.  The links to these are:

Mailing List: http://groups.google.com/group/qtile-dev

IRC: [irc://irc.oftc.net:6667/qtile](irc://irc.oftc.net:6667/qtile)

# Getting started

The first thing you will need to do to get started hacking on Qtile is to get
it installed.  Qtile has a small list of dependencies, primarily other Python
libraries, you can see the full guide to installing Python [in our
documentation](http://qtile.readthedocs.org/en/latest/manual/install/index.html).

Next, you will need to get yourself setup on GitHub.  We use git for version
control and our development takes place on GitHub.  Github has some [great help
pages](https://help.github.com/) to help get you setup with an account and
forking our [main repository](https://github.com/qtile/qtile/).  We have guides
for our recommended [contribution
workflow](http://qtile.readthedocs.org/en/latest/manual/contributing.html) and
on [hacking and
testing](http://qtile.readthedocs.org/en/latest/manual/hacking.html) the
changes you make.

Now that you are up and running, you can try to [configure
Qtile](http://qtile.readthedocs.org/en/latest/manual/config/index.html) to your
liking.  You can check out our [open
issues](https://github.com/qtile/qtile/issues) if you want somewhere to start
hacking, or feel free add functionality, make improvements, and fix bugs as you
see fit.  You can always [contact other developers](#contact-us) if you need
any help getting setup and running Qtile or would like to discuss your project.

# Writing your application

todo

# Project Ideas

Here are some potential ideas we have thought may be appropriate, however, feel
free to propose an idea to us that you are interested in.

## An asyncio-based DBus library.

As a part of the 0.9.0 release, we [dropped all C extensions as hard
dependencies](../blob/develop/CHANGELOG#L3), including dropping the PyGTK/GLib
event loop in favor of the new Python asyncio event loop.  However, several of
the widgets, such as the MPRIS widget, rely on D-Bus, and the [python-dbus
library](http://dbus.freedesktop.org/doc/dbus-python/) is [hard
coded](http://cgit.freedesktop.org/dbus/dbus-python/tree/dbus/mainloop/glib.py)
to use the gobject event loop.  We have come up with a
[hack](https://github.com/qtile/qtile/commit/82256a47b9f954b2bb9922b821dfd6d529dc5437)
to get around this limitation by wrapping the GLib event loop, however, it is
not ideal (in addition to being a bit of a hack, it means those widgets cannot
run in Qtile on PyPy).

The point of this project is to implement a D-Bus library that can work
directly with asyncio (and preferably other future event loops).  In other
words, this would be writing a pure Python D-Bus binding that can be configured
to fire off asyncio calls.  Tools such as cffi could be used to make the
necessary libdbus calls (this is what we did with
[xcffib](https://github.com/tych0/xcffib) to get rid of the xpyb dependency).

This project is not really Qtile specific, but will be needed by the general
python community going forward if the asyncio event loop is going take hold.

**Skills:**

Python, C (or at least C bindings in Python), and some understanding of how
D-Bus works.

**Difficulty:**

Medium/Hard. Although this will be very exciting because it will be a
from-scratch project, that means it will also be difficult because you will
have to design an entirely new codebase, including FFI to C.

**Related Links:**

* [Python asycio library](https://docs.python.org/3/library/asyncio.html)
* [gbulb](https://bitbucket.org/a_ba/gbulb), a Python 3.3+ library that wraps
  the GLib event loop in asyncio (but that we have not been able to get working
  with qtile)
* Current [python-dbus](http://dbus.freedesktop.org/doc/dbus-python/) C extension
* [CFFI](https://cffi.readthedocs.org/) could be used to call libdbus

## Experimental Wayland support

Wayland is the way forward, so it would be good to start working on
experimental support for it to see just how much of Qtile is re-usable.
The real value of Qtile is in the user contributed widgets, so we would like to
preserve that code going forward.

Preferably, we would be able to run Qtile as a standalone window manager, not
as a Weston shell.  This would require breaking out a lot of the current window
handling code into some abstraction layer that can be configured to run with
wither X or Wayland.  This would be a massive overhaul touching many points in
the codebase.

For this to work at all, we need Python bindings to libwayland, and maybe some
other Python libraries to do everything that needs to be done.  There is some
support for making language bindings for the Wayland
[server](http://cgit.freedesktop.org/wayland/wayland/commit/?id=c44090908db1c4f1b0e87bda2e4fdaa6bc15c0d1)
and
[client](http://cgit.freedesktop.org/wayland/wayland/commit/?id=eb947e9408c149041c4c8e1c80ef9ebea049f477),
but such bindings would may need to be implements. If you are interested in this
project, the first thing that should be addressed is what of the necessary tools
exist and what would need to be developed.

**Skills:**

Python, some understanding of Wayland (including the relevant C libraries
therein)

**Difficulty:**

Hard. Very few of us (if any) have any experience with Wayland, although we are
learning about the API. The student will likely need to take the lead on any
confusing Wayland issues and potentially work with their ML on any bugs that
arise.

**Related Links:**

* Resources available from Wayland http://wayland.freedesktop.org/

## Better layout serialization

Qtile currently does not serialize layout state across restarts and does not
maintain order and focus between layout changes, and it would be nice to have
that. For restarts, it may be possible to just pickle and pass the state of
each layout, but this would probably also involve a re-design of the layout
code to share some implementation pieces of the window shape(s).

**Skills:**

Python, good software design

**Difficulty:**

Easy. This one is really just an exercise in software design. Qtile's layout
code has grown over the years to incorporate lots of different options,
functions, overrides, etc., without anyone really sitting down to redesign
things. 

**Related Links:**

* Various bugs related to layouts:
  [#372](https://github.com/qtile/qtile/issues/549),
  [#473](https://github.com/qtile/qtile/issues/473),
  [#549](https://github.com/qtile/qtile/issues/549)

## Refactor window focusing

Properly routing focus and grabbing input is no simple task X, while Qtile is
able to properly deal with simple cases like using the mouse or keyboard to
select an open window, there are definitely places where we do not deal with
input appropriately.  Things like the [prompt
widget](http://qtile.readthedocs.org/en/latest/manual/ref/widgets.html#libqtile.widget.Prompt)
should be able to grab focus without other windows taking it back.  There are
also external panels which control window focus that do not work properly with
Qtile.  Just like the layout code, Qtile has adapted where it could, but it
would be great if the focusing could be overhauled to work correctly with
external programs and be extensible enough for things like the prompt widget to
function as expected.

**Skills:**

Python, good software design, maybe familiarity with X

**Difficulty:**

Easy/Medium.  Just like the layout project, a lot of this project will be
designing a same way to deal with layout that meets our needs.  This is a bit
trickier as it requires properly dealing with X events and input.

**Related Links:**

* Focusing issues coming up related to cursor warp:
  [#538](https://github.com/qtile/qtile/issues/538)
* Issue dragging windows with multiple screens
  [#297](https://github.com/qtile/qtile/issues/297)
* [Github issues mentioning
  focus](https://github.com/qtile/qtile/issues?q=focus+in%3Atitle+is%3Aissue+is%3Aopen),
  in particular [#444](https://github.com/qtile/qtile/issues/444)
* EWMH `_NET_ACTIVE_WINDOW`
  [hint](http://standards.freedesktop.org/wm-spec/latest/ar01s03.html#idm139870830075168)
* ICCCM [discussion of input
  focus](http://tronche.com/gui/x/icccm/sec-4.html#s-4.1.7)

## Quick Jump to Windows in a Group by Window Title Search 

This would be very useful if you have a lot of windows in a group and you want
to focus quickly to the current window without navigating trough the panes / or
zapping trough application in the maximum-size layout. Imagine, you can type /f
to get quickly to the open Firefox window in the group (like in VIM jumping to
a text passage), and so on. Let us discuss this idea!

**Skills:**

**Difficulty:**

**Related Links:**

# Possible Mentors