## Floating windows
Here are some snippets of code related to floating windows.

- [Bring all floating windows to front](#bring-all-floating-windows-to-front)
- [Set specific windows in floating mode](#set-specific-windows-in-floating-mode)
  - [Any window](#any-window)
  - [Firefox](#firefox)
  - [PyCharm](#pycharm)
  - [Steam](#steam)

### Bring all floating windows to front
Sometimes a floating window ends up behind another window,
and since the floating layout is separated from the other layouts,
chances are you cannot focus the window back to front.

So here is a lazy function that will bring **all** floating windows to front:

```python
@lazy.function
def float_to_front(qtile):
    logging.info("bring floating windows to front")
    for group in qtile.groups:
        for window in group.windows:
            if window.floating:
                window.cmd_bring_to_front()
```

Now simply define a key that uses this function:

```python
keys = [
    ...
    Key(["mod4", "shift"], "f", float_to_front),
    ...
]
```

### Set specific windows in floating mode
We currently have snippets for the following applications:
- [Firefox](#firefox)
- [PyCharm](#pycharm)
- [Steam](#steam)

#### Any window
If the application you search for is not referenced in this wiki,
here are methods to know how to make it floating.
The goal is to find the **uniquely identifying information**
of the application window.

<details>
<summary><strong>With xprop</strong></summary><br>

1. install the `xprop` tool if it's not already available on your system
2. launch the desired application
3. run `xprop` in a terminal
4. click on the application window

In some cases, the window you want to have floating disappear quite fast.
In that case, you'll want to run a command like `application & xprop`.
Be prepared to click quickly!

Now that you have clicked on the application window,
`xprop` exits and you're left with some information output in your terminal.
You need to find the **identifying information** in this output.

Some applications are difficult to uniquely identify,
like the Update window of [Steam](#steam).
Don't hesitate to run a `diff` on the `xprop` output
of two different windows of the same application
to find identifying information.

You will usually look at `WM_CLASS` and `WM_NAME`. As an example,
here is the output of `xprop` for an Atom (the editor) window:

```
XdndTypeList(ATOM) = STRING, UTF8_STRING, TEXT, text/plain, chromium/x-renderer-taint, chromium/x-web-custom-data
_NET_WM_STATE(ATOM) = _NET_WM_STATE_MAXIMIZED_VERT, _NET_WM_STATE_MAXIMIZED_HORZ
_NET_WM_DESKTOP(CARDINAL) = 0
WM_STATE(WM_STATE):
		window state: Normal
		icon window: 0x0
_NET_WM_USER_TIME(CARDINAL) = 105925334
WM_NORMAL_HINTS(WM_SIZE_HINTS):
		program specified location: 1080, 440
_NET_WM_ICON(CARDINAL) =
WM_NAME(UTF8_STRING) = "custom-apps.md — ~/data/dev/forks/qtile-wiki — Atom"
_NET_WM_NAME(UTF8_STRING) = "custom-apps.md — ~/data/dev/forks/qtile-wiki — Atom"
XdndAware(ATOM) = BITMAP
_MOTIF_WM_HINTS(_MOTIF_WM_HINTS) = 0x2, 0x0, 0x1, 0x0, 0x0
_NET_WM_BYPASS_COMPOSITOR(CARDINAL) = 2
WM_WINDOW_ROLE(STRING) = "browser-window"
WM_CLASS(STRING) = "atom", "Atom"
_NET_WM_WINDOW_TYPE(ATOM) = _NET_WM_WINDOW_TYPE_NORMAL
_NET_WM_PID(CARDINAL) = 1132137
WM_LOCALE_NAME(STRING) = "en_US.UTF-8"
WM_CLIENT_MACHINE(STRING) = "corsair"
WM_PROTOCOLS(ATOM): protocols  WM_DELETE_WINDOW, _NET_WM_PING
```
</details>

<details>
<summary><strong>With qtile-cmd -o window -f inspect</strong></summary><br>

To get information about the desired application window,
prepare to quickly focus the window
(by moving your mouse or by your keyboard shortcuts),
and run this one-liner in a terminal:

```bash
sleep 1; qtile-cmd -o window -f inspect
```

You will have one second to move the focus to the desired window.
Increase the sleep time if it is too short.

Example output for an Atom window:

```
{'attributes': {'all_event_masks': 6520959,
                'backing_pixel': 0,
                'backing_planes': 4294967295,
                'backing_store': 0,
                'bit_gravity': 1,
                'class': 1,
                'do_not_propagate_mask': 0,
                'map_is_installed': 1,
                'map_state': 2,
                'override_redirect': 0,
                'save_under': 0,
                'visual': 33,
                'win_gravity': 1,
                'your_event_mask': 6422544},
 'float_info': {'height': 1896, 'width': 1080, 'x': 0, 'y': 24},
 'hints': None,
 'name': 'custom-apps.md — ~/data/dev/forks/qtile-wiki — Atom',
 'normalhints': {'base_height': 0,
                 'base_width': 0,
                 'flags': {'PPosition'},
                 'height_inc': 0,
                 'max_aspect': 0,
                 'max_height': 0,
                 'max_width': 0,
                 'min_aspect': 0,
                 'min_height': 0,
                 'min_width': 0,
                 'width_inc': 0,
                 'win_gravity': 0},
 'properties': ['XdndTypeList',
                '_NET_WM_STATE',
                '_NET_WM_DESKTOP',
                'WM_STATE',
                '_NET_WM_USER_TIME',
                'WM_NORMAL_HINTS',
                '_NET_WM_ICON',
                'WM_NAME',
                '_NET_WM_NAME',
                'XdndAware',
                '_MOTIF_WM_HINTS',
                '_NET_WM_BYPASS_COMPOSITOR',
                'WM_WINDOW_ROLE',
                'WM_CLASS',
                '_NET_WM_WINDOW_TYPE',
                '_NET_WM_PID',
                'WM_LOCALE_NAME',
                'WM_CLIENT_MACHINE',
                'WM_PROTOCOLS'],
 'protocols': ['WM_DELETE_WINDOW', '_NET_WM_PING'],
 'state': (1, 0),
 'wm_class': ('atom', 'Atom'),
 'wm_client_machine': 'corsair',
 'wm_icon_name': None,
 'wm_transient_for': None,
 'wm_type': 'normal',
 'wm_window_role': 'browser-window'}
```

You need to find the **identifying information** in this output.

Some applications are difficult to uniquely identify,
like the Update window of [Steam](#steam).
Don't hesitate to run a `diff` on the command output
of two different windows of the same application
to find identifying information.

You will usually look at `wm_class` and `name`.
</details><br>

Now in your hook function, you can retrieve the window information
with methods of the `window` attribute. Example:

```python
wm_class = window.window.get_wm_class()
...
```

:warning: | TODO: complete mapping of variables names in inspect output and window methods.
---: | :----

:warning: | TODO: complete mapping of lines in `xprop` output and window methods.
---: | :----


Now, to force a window to float, you can use the `client_new` hook.
See [Hooks](http://docs.qtile.org/en/latest/manual/ref/hooks.html)
for more information on hooks.

```python
from libqtile import hook


@hook.subscribe.client_new
def float_my_app(window):
    if window.window.get_name() == "My App":
        window.floating = True
```

Also see examples below.


#### Firefox
You typically want the secondary windows of Firefox,
like the download history, the bookmark managers and else,
to be floating:

```python
from libqtile import hook


@hook.subscribe.client_new
def float_firefox(window):
    wm_class = window.window.get_wm_class()
    w_name = window.window.get_name()
    if wm_class == ("Places", "firefox") and w_name == "Library":
        window.floating = True
```

#### PyCharm
You typically want to float the small brand/intro window
and the updater windows:

```python
from libqtile import hook


@hook.subscribe.client_new
def float_pycharm(window):
    wm_class = window.window.get_wm_class()
    w_name = window.window.get_name()
    if (
        (
            wm_class == ("jetbrains-pycharm-ce", "jetbrains-pycharm-ce")
            and w_name == " "
        )
        or (
            wm_class == ("java-lang-Thread", "java-lang-Thread")
            and w_name == "win0"
        )
    ):
        window.floating = True
```

#### Steam
You typically want to put **every** window that is not the main one
in floating mode:

```python
from libqtile import hook


@hook.subscribe.client_new
def float_steam(window):
    wm_class = window.window.get_wm_class()
    w_name = window.window.get_name()
    if (
        wm_class == ("Steam", "Steam")
        and (
            w_name != "Steam"
            # w_name == "Friends List"
            # or w_name == "Screenshot Uploader"
            # or w_name.startswith("Steam - News")
            or "PMaxSize" in window.window.get_wm_normal_hints().get("flags", ())
        )
    ):
        window.floating = True
```
