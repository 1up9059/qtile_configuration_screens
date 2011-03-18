I'm wondering How qtile should handle having no groups (empty groups list),
at present, qtile isn't too happy about having no groups.<br>
I can see three possible directions:<br>
1) have any functions that manipulate the groups list refuse to remove the last group.
2) automatically spawn a new group should the group list become empty.
3) have qtile happily handle an empty group list.

I prefer the last option, but any function that uses the groups list what have to handle
the possibility of it being empty e.g. widgets that display the name of the current group
might have to show some other piece of text.

the second option could be implemented in much the same way, except the situation is always
handled by spawning a new group. Another option would be that any function that removes a
group from the group list check (before or after) that the group (would be/is not) empty, and
if so, spawn a new group.

Finally, the first option would do much the same, except instead of spawning a group, the
function would just refuse to remove the last group.

Please give me your feedback.
 -- Chris2048
