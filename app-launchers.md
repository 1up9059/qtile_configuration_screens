## Applications launchers
This page shows how to create application launchers with keyboard shortcuts.

- [Dmenu apps](#dmenu-apps)
- [Dropdown terminal](#dropdown-terminal)


### Dmenu apps

:information_source: | Code taken from [qtile/qtile-examples/zordsdavini](https://github.com/qtile/qtile-examples/tree/master/zordsdavini).
---: | :----

Here is an example of how to run `dmenu` apps:

```python
class Commands:
    dmenu = "dmenu_run -i -b -p '>>>' -fn 'Open Sans-10' -nb '#000' -nf '#fff' -sb '#00BF32' -sf '#fff'"
    dmenu_session = "dmenu-session"
    dmenu_mocp = "dmenu-mocp"
    dmenu_windows = "dmenu-qtile-windowlist.py"


mod = "mod4"
keys = [
    Key([mod, "control"], "q", lazy.spawn(Commands.dmenu_session)),
    Key([mod], "m", lazy.spawn(Commands.dmenu_mocp)),
    Key([mod], "l", lazy.spawn(Commands.dmenu_windows)),
    Key([mod], "r", lazy.spawn(Commands.dmenu)),
]
```

### Dropdown terminal

:information_source: | Code taken from [qtile/qtile-examples/mort65](https://github.com/qtile/qtile-examples/tree/master/mort65).
---: | :----

Here is a snippet of code that shows how to setup a dropdown terminal:

```python
from libqtile.config import ScratchPad, DropDown


# define your keys and groups
keys = [...]
groups = [...]

# append a scratchpad group
groups.append(
    ScratchPad(
        "scratchpad", [
            # define a drop down terminal
            # it is placed in the upper third of the screen by default
            DropDown(
                "term",
                "/usr/bin/termite",
                opacity=0.88,
                height=0.55,
                width=0.80
            ),

            # define another terminal exclusively for qshell at different position
            DropDown(
                "qshell",
                "/usr/bin/termite -e qshell",
                x=0.05,
                y=0.4,
                width=0.9,
                height=0.6,
                opacity=0.9,
                on_focus_lost_hide=True
            )
        ]
    )
)

# define keys to toggle the dropdown terminals
keys.extend([
    Key([], "F12", lazy.group["scratchpad"].dropdown_toggle("term")),
    Key([], "F11", lazy.group["scratchpad"].dropdown_toggle("qshell")),
])
```
