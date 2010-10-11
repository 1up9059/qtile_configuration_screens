

The disablemask method in the window could alternatively be a contextmanager.
This way you cannot forget to call resetmask.

for clarity, all of the "protected" type members should have in their docstring
where they intend to be called from. Furthermore, all of the handle_* type
methods, which are not explicitely called except via introspection should have a
reference for how the introspection process happens.

bar.py does some tricky stuff when configuring its underlying window. This is
helpful for setting up other internal windows, but perhaps there should be a
better system for doing it?..

