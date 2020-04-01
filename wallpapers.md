## Wallpapers

This page shows how to setup your wallpaper with Qtile.

- [Natively](#native-setting)
- [With nitrogen](#nitrogen)
- [With feh](#feh)
- [With the wallpaper widget](#wallpaper-widget)

### Native setting

Wallpapers can be set in your config file as parameters to `Screen` instances:

```python
screens = [
    Screen(
            wallpaper='~/path/to/my/wallpaper.png',
            wallpaper_mode='fill',
        )
]
```

Available wallpaper modes:

 - `None` (default): The image will be placed at the screens origin and retain its own dimensions.
 - `'fill'`: The image will be centred on the screen and resized to fill it while maintaining aspect ratio.
 - `'stretch'`: The image is stretched to fit all of it into the screen.

### Nitrogen

In your [autostart](http://docs.qtile.org/en/latest/manual/config/hooks.html#autostart) script:

```bash
nitrogen --restore &
```

### Feh

In your [autostart](http://docs.qtile.org/en/latest/manual/config/hooks.html#autostart) script:

```bash
feh --bg-scale ~/path/to/your/wallpaper.jpg &
```

### Wallpaper widget

Qtile provides a [Wallpaper widget](http://docs.qtile.org/en/latest/manual/ref/widgets.html#wallpaper).
