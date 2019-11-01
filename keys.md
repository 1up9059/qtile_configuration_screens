## Keyboard keys
This page shows code snippets related to keyboard keys configuration.

- [Enum classes that hold keys](#enum-classes-that-hold-keys)
- [Multimedia keys](#multimedia-keys)
  - [Audio keys](#audio-keys)
  - [Brightness](#brightness)
  - [Screenshots](#screenshots)
    - [Maim](#maim)
    - [Xfce4-screenshot](#xfce4-screenshot)
- [Using the Hyper key](#using-the-hyper-key)

### Enum classes that hold keys
While waiting for the snippet in
[#1449](https://github.com/qtile/qtile/issues/1449)
to make its way into the source code,
maybe you experienced configuration errors
because you typed a key wrong.

For example, you typed "left" instead of "Left",
reloaded your configuration,
and boom, nothing is working anymore.
And you cannot validate your configuration file before hand
by running it through Python,
because key values are not immediately checked by Qtile
when loading the configuration file, so it fails a bit later
when validating the keys.

One way to avoid that is to use a enumeration for keys.
If the variable you typed does not exist,
it will immediately trigger a Python `AttributeError`,
allowing you to validate your configuration file
by running it through Python, with `python /path/to/config.py`.

Here is a snippet of code you can use to do that:

```python
from libqtile.xkeysyms import keysyms


class KeysHolder:
    def __init__(self):
        # set attribute for every key found in Qtile's keysyms
        for key in keysyms.keys():
            self_key = key.upper()
            if key[0] in range(0, 9):
                # for numbers, prepend K
                self_key = "K" + self_key
            setattr(self, self_key, key)

        # special aliases
        self.ALT = self.MOD1 = "mod1"
        self.HYPER = self.MOD3 = "mod3"
        self.SUPER = self.MOD4 = "mod4"
        self.SHIFT = "shift"
        self.CONTROL = "control"
        self.EXCLAMATION = self.EXCLAM
        self.DOUBLE_QUOTE = self.QUOTEDBL


class MouseButtons:
    LEFT = BUTTON1 = "Button1"
    MIDDLE = BUTTON2 = "Button2"
    RIGHT = BUTTON3 = "Button3"
    WHEEL_UP = BUTTON4 = "Button4"
    WHEEL_DOWN = BUTTON5 = "Button5"
    WHEEL_LEFT = BUTTON6 = "Button6"
    WHEEL_RIGHT = BUTTON7 = "Button7"
    PREVIOUS = BUTTON8 = "Button8"
    NEXT = BUTTON9 = "Button9"


k = KeysHolder()
m = MouseButtons()
```

Now you can define your keys like so:

```python
keys = [
  Key([k.SUPER], k.LEFT, lazy.layout.left()),
  Key([k.SUPER], k.UP, lazy.layout.up()),
  Key([k.SUPER], k.DOWN, lazy.layout.down()),
  Key([k.SUPER], k.RIGHT, lazy.layout.right()),
  ...
  # of course for simple letters, use them literally
  Key([k.SUPER], "w", lazy.window.kill()),
]

mouse = [
    Drag(
        [k.ALT],
        m.LEFT,
        lazy.window.set_position_floating(),
        start=lazy.window.get_position(),
    ),
    ...
]
```

### Multimedia keys
To find the name of the multimedia keys in Qtile, go to the
[libqtile/xkeysyms.py](https://github.com/qtile/qtile/blob/master/libqtile/xkeysyms.py)
page.

#### Audio keys
You can use `amixer` and `mpc` to define the actions of your audio keys:

```python
keys = [
    Key([], "XF86AudioNext", lazy.spawn("mpc next")),
    Key([], "XF86AudioPrev", lazy.spawn("mpc prev")),
    Key([], "XF86AudioPlay", lazy.spawn("mpc toggle")),
    Key([], "XF86AudioStop", lazy.spawn("mpc stop")),

    # general volume
    Key([], "XF86AudioRaiseVolume", lazy.spawn("amixer -c 0 -q set Master 2dB+")),
    Key([], "XF86AudioLowerVolume", lazy.spawn("amixer -c 0 -q set Master 2dB-")),

    # music volume
    Key(["mod4"], "XF86AudioRaiseVolume", lazy.spawn("mpc volume +5")),
    Key(["mod4"], "XF86AudioLowerVolume", lazy.spawn("mpc volume -5")),
    ...
]
```

Of course `mpc` will only work if you are using `mpd` (Music Player Daemon)
as your music player.

#### Brightness
You can use an utility like `brightnessctl` to define the actions of your brightness keys:
```python
keys = [
    Key([], "XF86MonBrightnessUp", lazy.spawn("brightnessctl set +2%")),
    Key([], "XF86MonBrightnessDown", lazy.spawn("brightnessctl set 2%-")),
    ...
]
```

:information_source: | NOTE: `brightnessctl` have an udev rule to allow users in `video` group write the brightness file. You can add your user to `video` group with `sudo usermod -aG video $USER`.
---: | :---

:information_source: | NOTE: if `brightnessctl` must be run as root, you can set the SUID bit on the executable with `sudo chmod u+s /usr/bin/brightnessctl`.
---: | :---

#### Screenshots
You can use an screenshot taker as `maim`, `scrot`, even `xfce4-screenshoter` to define the actions of your `Print` key.

#### Maim
```python
keys = [
    # Full screen
    Key(["mod4"], "Print", lazy.spawn("maim -u ~/Pictures/screenshot/screen_$(date +%Y-%m-%d-%T).png")),
    # Select area
    Key(["mod4", "shift"], "Print", lazy.spawn("maim -s ~/Pictures/screenshot/area_$(date +%Y-%m-%d-%T).png")),
    # Active window
    Key(["mod4", "control"], "Print",
        lazy.spawn("maim -u -i $(xdotool getactivewindow) ~/Pictures/screenshot/window_$(date +%Y-%m-%d-%T).png")),
    ...
]
```

Active window binding will require `xdotool`.

#### Xfce4-screenshot
```python
keys = [
    # Open GUI
    Key([], "Print", lazy.spawn("xfce4-screenshooter")),
    # Full screen
    Key(["mod4"], "Print", lazy.spawn("xfce4-screenshooter -f")),
    # Select area
    Key(["mod4", "shift"], "Print", lazy.spawn("xfce4-screenshooter -r")),
    # Active window
    Key(["mod4", "control"], "Print", lazy.spawn("xfce4-screenshooter -w")),
    ...
]
```

:warning: | TODO: add keys for keyboard lights, etc.
---: | :----

### Using the Hyper key
To replace the `CapsLock` key by the Hyper key,
which is called `mod4`, put these contents in `~/.Xmodmap`:

```
clear Lock
keycode 66 = Hyper_L
remove mod4 = Hyper_L
add mod3 = Hyper_L
```

Make sure you run `xmodmap ~/.Xmodmap` at some point during startup,
either from your `~/.xinitrc` file or from a Qtile hook,
for example in the `startup_once` hook.
See [Hooks](http://docs.qtile.org/en/latest/manual/ref/hooks.html)
for more information on hooks.
Also see this issue if you are having issues:
[#1414](https://github.com/qtile/qtile/issues/1414).
