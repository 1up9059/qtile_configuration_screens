I'm wondering How qtile should handle having no groups (empty groups list),<br>
at present, qtile isn't too happy about having no groups.<br>
I can see three possible directions:<br>
1) have any functions that manipulate the groups list refuse to remove the last group.<br>
2) automatically spawn a new group should the group list become empty.<br>
3) have qtile happily handle an empty group list.<br><br>
I prefer the last option, but any function that uses the groups list what have to handle<br>
the possibility of it being empty e.g. widgets that display the name of the current group<br>
might have to show some other piece of text.<br><br>
the second option could be implemented in much the same way, except the situation is always<br>
handled by spawning a new group. Another option would be that any function that removes a<br>
group from the group list check (before or after) that the group (would be/is not) empty, and<br>
if so, spawn a new group.<br><br>
Finally, the first option would do much the same, except instead of spawning a group, the<br>
function would just refuse to remove the last group.<br>
<br>Please give me your feedback.<br>
 -- Chris2048
