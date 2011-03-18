I made quite a few changes to the 'group' object in one of my branches. When it was time to<br>
merge with changes added by others, I felt I was doing a lot of un-necessary work born of the<br>
fact that there is a lot of stuff in 'manager.py' that is by-and-large unrelated, but otherwise<br>
core, and quite often updated.<br><br>
I propose things 'screen' and 'group' objects get their own directories/files (much like<br>
'layout' and 'window') to make it easier to work on them. I also tried doing this and<br>
noticed that it was also an advantage to clarify what dependencies are needed for each<br>
file ans its objects.<br>
 --Chris2048<br>
