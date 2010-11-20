lo. Here's my config.py as of 20th Nov 2010:

```python
# ~/.config/qtile/config.py

from libqtile.manager import Key, Screen, Group
from libqtile.command import lazy
from libqtile import hook, layout, bar, widget
from libqtile.confreader import *

screens = [
    Screen(
        bottom =
        bar.Bar([
                widget.AGroupBox(),
                widget.WindowName(),
                widget.Clock()],
                28),)]
add_screens(screens)

modkey = "mod4"
keys = [
    # First, a set of bindings to control the layouts
#    Key([modkey],               "p",        lazy.spawn("exe=`dmenu_path | dmenu` && eval \"exec $exe\"")),
    Key([modkey],               "j",        lazy.layout.up()),
    Key([modkey],               "k",        lazy.layout.down()),
    Key([modkey],               "l",        lazy.group.nextgroup()),
#    Key([modkey],               "p",        lazy.widget['prompt'].start_input("XX: ","print", "cmd")),
    Key([modkey],               "n",        lazy.spawn("chromium")),
    Key([modkey],               "u",        lazy.spawn("uzbl-browser")),
    Key([modkey],               "e",        lazy.spawn("tmux neww -t default 'emacsclient -nca=''; read'")),
    Key([modkey],               "Return",   lazy.spawn("myxterm scr-ssh")),
    Key([modkey],               "v",        lazy.spawn("myxterm -bg grey40 vim")),
    Key([modkey],               "q",        lazy.spawn("sudo killall qtile")),
    Key([modkey],               "i",        lazy.shutdown()),
    Key([modkey],               "r",        lazy.reload()),
    Key([modkey],               "Tab",      lazy.nextlayout()),
    Key([modkey],               "w",        lazy.window.kill()),
#--- sublayouts
#    Key([modkey, "shift"],      "Tab",      lazy.layout.nextsublayout()),
#    Key(["mod1"], "Up", lazy.layout.command_sublayout('*', 'incratio', 0.02)),
#    Key(["mod1"], "Down", lazy.layout.command_sublayout('*', 'incratio', -0.02)),
#    Key([modkey], "m", lazy.layout.command_sublayout('*', 'incnmaster')),
#    Key([modkey, "shift"], "m", lazy.layout.command_sublayout('*', 'incnmaster', -1))
]
add_keys(keys)

#theme = Theme( **{ 'fg_normal': '#989898',
#                   'fg_focus': '#00d691',
#                   'fg_active': '#ffffff',
#                   'bg_normal': '#181818',
#                   'bg_focus': '#252525',
#                   'bg_active': '#181818',
#                   'border_normal': '#181818',
#                   'border_focus': '#0096d1',
#                   'border_width': 5,
#                   'font': 24 })
#'-*-zekton-*-r-normal-*-14-*-*-*-*-0-*-*'})
# 'ttffont': '/home/ben/.fonts/Diavlo_MEDIUM_II_37.ttf', 'ttffontsize': 20})

#    specials = {'magnify': {'border_width': 5,}, 'bar': {'opacity': 0.8,},})
#'ttffont': '/usr/share/fonts/TTF/zektonbo.ttf' '/usr/share/fonts/TTF/eurofc35.ttf',

# Two simple layout instances:
layouts = [
    layout.Max(),
#    layout.ClientStack(
#        [
#            (layout.SubTile, {}),
#            (layout.SubMagnify, {}),
#            (layout.SubVertTile, {}),
#            (layout.SubMax, {}),
#            (layout.SubFloating, {}),
#            (layout.SubTile, {'expand': False})],
#        focus_mode = layout.ClientStack.FOCUS_TO_LAST_FOCUSED,
#        ),
    layout.Stack(stacks=2)]
add_layouts(layouts)

def main(q):
    import re
    from libqtile import hook

    class Match(object):
        ''' Match for dynamic groups
            it can match by title, class or role '''
        def __init__(self, title=[], wm_class=[], role=[]):
            self._rules = [('title', t) for t in title]
            self._rules += [('wm_class', w) for w in wm_class]
            self._rules += [('role', r) for r in  role]

        def compare(self, client):
            for _type, rule in self._rules:
                match_func = getattr(rule, 'match', None) or\
                             getattr(rule, 'count')
                if _type == 'title':
                    value = client.name
                elif _type == 'wm_class':
                    value = client.window.get_wm_class()[1]
                else:
                    value = client.window.get_wm_window_role()

                if match_func(value):
                    return True
            return False

    class DGroups(object):
        ''' Dynamic Groups '''
        def __init__(self, qtile, groups, apps, config=None):
            self.qtile = qtile

            self.groups = groups
            self.apps = apps

            self.config = config

            self._setup_hooks()
            self._setup_groups()

        def _setup_groups(self):
            for name, tag in self.groups.iteritems():
                spawn_cmd = tag.get('spawn')
                # have proper 'spawn' mechanism, compatible with keypress objects?
                # also, why associate with a group, won't matching do this? A
                # better way is spawn via main(), and associate spawned with groups.
                # maybe add 'one-off' groups, or groups with a set limit of clients?
                if spawn_cmd:
                    self.qtile.cmd_spawn(spawn_cmd)

        def _setup_hooks(self):
            hook.subscribe.client_new(self._add)
            hook.subscribe.client_killed(self._del)

        def _add(self, client):
            for app in self.apps:
                # Matching Rules
                if app['match'].compare(client):
                    group = app['group']
                    self.qtile.addGroup(group)
                    client.togroup(group)
                    return

            # Unmatched
            current_group = self.qtile.currentGroup.name
            if current_group in self.groups and\
                    self.groups.get(current_group, 'exclusive'):
                if not 'default' in self.groups:
                    self.qtile.addGroup('default')
                client.togroup('default')

        def _del(self, client):
            group = client.group

            # Delete group if empty and no persist
            if not (group.name in self.groups and\
               self.groups[group.name].get('persist')) and\
                                   len(group.windows) == 1:
                self.qtile.delGroup(group.name)

    groups = {
        # init: have initial window, dupes? I replaced with 'default' group
        # persist: exists even with no clients
        # spawn: automatically spawn client?
        # exclusive: don't let unmatched clients show here
            'term':  { 'exclusive': True },
            'uzbl':  { 'exclusive': True },
            'edit':  { 'exclusive': True },
           }
    apps = [
            {'match': Match(wm_class=['XTerm']), 'group': 'term'},
            {'match': Match(wm_class=['Chrome']), 'group': 'uzbl'},
            {'match': Match(wm_class=['Uzbl-*']), 'group': 'uzbl'},
            {'match': Match(wm_class=['Emacs']), 'group': 'edit'}
           ]

    dgroups = DGroups(q, groups, apps)

add_main(main)
```