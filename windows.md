## Windows
This page shows code snippets related to Qtile windows.

- [Resizing windows](#resizing-windows)
  - [Two-in-one grow/shrink for BSP layout](#two-in-one-growshrink-for-BSP-layout)

### Resizing windows
You will find here some snippets related to resizing windows.

#### Two-in-one grow/shrink for BSP layout
If like me you are used to
[Terminator](https://launchpad.net/terminator)'s way of resizing panes,
you may find this snippet interesting.
It allows you to resize a window, growing it or shrinking it,
without changing the focus to a neighbor.
See this feature request: [#1402](https://github.com/qtile/qtile/issues/1402).

:warning: | TODO: only tested under BSP, check if it works for other layouts.
---: | :----

The main logic is written in a `resize` function:

```python
def resize(qtile, direction):
    layout = qtile.current_layout
    child = layout.current
    parent = child.parent

    while parent:
        if child in parent.children:
            layout_all = False

            if (direction == "left" and parent.split_horizontal) or (
                direction == "up" and not parent.split_horizontal
            ):
                parent.split_ratio = max(5, parent.split_ratio - layout.grow_amount)
                layout_all = True
            elif (direction == "right" and parent.split_horizontal) or (
                direction == "down" and not parent.split_horizontal
            ):
                parent.split_ratio = min(95, parent.split_ratio + layout.grow_amount)
                layout_all = True

            if layout_all:
                layout.group.layout_all()
                break

        child = parent
        parent = child.parent
```

Then you can easily define lazy functions to be used with keys:

```python
@lazy.function
def resize_left(qtile):
    resize(qtile, "left")


@lazy.function
def resize_right(qtile):
    resize(qtile, "right")


@lazy.function
def resize_up(qtile):
    resize(qtile, "up")


@lazy.function
def resize_down(qtile):
    resize(qtile, "down")


keys = [
    ...
    Key(["control", "shift"], "Left", resize_left),
    Key(["control", "shift"], "Up", resize_up),
    Key(["control", "shift"], "Down", resize_down),
    Key(["control", "shift"], "Right, resize_right),
    ...
]
```