## Workspaces
This page shows code snippets allowing you to simulate
"classic" workspaces, such as in XFCE window manager.

Here is how to simulate Xubuntu-like workspaces in Qtile.
You will have four workspaces, that you can cycle through
with `Ctrl`+`Alt`+`Left`/`Right`.

You will also be able to move a window while cycling
through workspaces with `Ctrl`+`Alt`+`Shift`+`Left`/`Right`.

```python
from libqtile.lazy import lazy  # on qtile 0.14: from libqtile.command import lazy


def cycle_workspaces(direction, move_window):
    def _inner(qtile):
        current = qtile.groups.index(qtile.current_group)
        destination = (current + direction) % len(groups)
        if move_window:
            qtile.current_window.togroup(qtile.groups[destination].name)
        qtile.groups[destination].cmd_toscreen()
    return _inner


next_workspace = lazy.function(cycle_workspaces(direction=1, move_window=False))
previous_workspace = lazy.function(cycle_workspaces(direction=-1, move_window=False))
to_next_workspace = lazy.function(cycle_workspaces(direction=1, move_window=True))
to_previous_workspace = lazy.function(cycle_workspaces(direction=-1, move_window=True))


keys = [
    Key(["control", "mod1"], "Right", next_workspace),
    Key(["control", "mod1"], "Left", previous_workspace),
    Key(["control", "mod1", "shift"], "Right", to_next_workspace),
    Key(["control", "mod1", "shift"], "Left", to_previous_workspace),
]
```

Credits to @m-col: https://gist.github.com/m-col/4f96e9c1b417574a68c6787e1f825857, thanks!