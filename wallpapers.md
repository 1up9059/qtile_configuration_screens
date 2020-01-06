## Wallpapers

This page shows how to setup your wallpaper with Qtile.

- [With nitrogen](#nitrogen)
- [With feh](#feh)
- [With the wallpaper widget](#wallpaper-widget)

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
