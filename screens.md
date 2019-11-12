## Screens
This page shows code snippets related to screens.

- [Setup multiple screens dynamically](#setup-multiple-screens-dynamically)

### Setup multiple screens dynamically
If you share your configuration between different setups,
you might want to dynamically set your screens, whatever their number.

To do this, you have to know how many screens are connected and used.
This is done using `Xlib`. You need to install `python-xlib` in the same
environment as Qtile.

```python
# this import requires python-xlib to be installed
from Xlib import display as xdisplay


def get_num_monitors():
    num_monitors = 0
    try:
        display = xdisplay.Display()
        screen = display.screen()
        resources = screen.root.xrandr_get_screen_resources()

        for output in resources.outputs:
            monitor = display.xrandr_get_output_info(output, resources.config_timestamp)
            preferred = False
            if hasattr(monitor, "preferred"):
                preferred = monitor.preferred
            elif hasattr(monitor, "num_preferred"):
                preferred = monitor.num_preferred
            if preferred:
                num_monitors += 1
    except Exception as e:
        # always setup at least one monitor
        return 1
    else:
        return num_monitors

num_monitors = get_num_monitors()
```

Now, you may want to define each screen manually,
or just define the main screen and setup the other equally.

Here is the latter:

```python
screens = [
    Screen(
        top=bar.Bar(
            [...],  # main screen widgets
            bar_size=24,
            opacity=0.8,
        ),
    )
]

if num_monitors > 1:
    for m in range(num_monitors - 1):
        screens.append(
            Screen(
                top=bar.Bar(
                    [...],  # other screens widgets
                    bar_size=24,
                    opacity=0.8,
                ),
            )
        )
```
