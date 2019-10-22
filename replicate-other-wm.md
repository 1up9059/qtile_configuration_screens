## Replicate other window managers
On this page you will see how to replicate
(entirely or partially) the behavior of
other (tiling) window managers.

:warning: | Work in progress! (last update 22/10/2019).
---: | :----

- [i3](#i3)
- [Special: Terminator](#special-terminator)

### i3

:warning: | TODO: replicate i3 / i3-gaps.
---: | :----

### Special: Terminator
If you are used to the way [Terminator](https://launchpad.net/terminator) splits panes,
the BSP layout ([see docs](http://docs.qtile.org/en/latest/manual/ref/layouts.html#bsp)) is for you.

First, lets define the layout itself:

```python
from libqtile import layout


layouts = [
    ...
    layout.Bsp(
        border_normal="#000000",
        border_focus="#ffffff",
        border_width=1,
        fair=False,
        lower_right=False,
        grow_amount=1,
        # uncomment if you want gaps between windows
        #margin=20,
    ),
    ...
]
```

:warning: | TODO: replicate Terminator keys (and add link to related resizing section)
---: | :----