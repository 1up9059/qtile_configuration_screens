## Troubleshooting
This page contains solutions for common problems.

- [Java applications display issues](#java-applications-display-issues)

### Java applications display issues
Applications that are built with Java may sometimes not load correctly.
If it happens, it might be because you changed the window manager name
at the end of your configuration file:

```python
# XXX: Gasp! We're lying here. In fact, nobody really uses or cares about this
# string besides java UI toolkits; you can see several discussions on the
# mailing lists, GitHub issues, and other WM documentation that suggest setting
# this string if your java app doesn't work correctly. We may as well just lie
# and say that we're a working one by default.
#
# We choose LG3D to maximize irony: it is a 3D non-reparenting WM written in
# java that happens to be on java's whitelist.
wmname = "LG3D"
```

Setting `wmname = "LG3D"` will allow Java application to load correctly.

The (minor) problem with using this `"LG3D"` string
is that `neofetch` or similar tools will use it to name
your window manager.
If like me you are proud of using Qtile,
you really want `neofetch` to print `qtile` and not something else.

Well, there is a solution.

You have to export the environment variable `_JAVA_AWT_WM_NONREPARENTING`
with a value of `1` before launching your Java application.

In Bash it is done with `export _JAVA_AWT_WM_NONREPARENTING=1`.

You can either export this variable in the beginning of a wrapper script
that launches the Java application, or export it globally when the system
starts up.

For example, see this `/usr/bin/pycharm` wrapper script:

```bash
#!/usr/bin/env bash
export _JAVA_AWT_WM_NONREPARENTING=1
/path/to/pycharm-community-2019.2.1/bin/pycharm.sh
```

For more information,
see issues [#558](https://github.com/qtile/qtile/issues/558)
and [#1189](https://github.com/qtile/qtile/issues/1189)
