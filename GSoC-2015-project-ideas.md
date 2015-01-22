1. An asyncio-based DBus library. This is not really qtile specific,
   but will be needed by the general python community going forward.
   Having something like this would also get rid of our hack to work
   around not having it.

2. Experimental Wayland support. Wayland is the way forward, so it would be
   good to start working on experimental support for it to see just how much
   of qtile is re-usable. The real value of qtile is in the user contributed
   wigets, so we'd like to preserve that code going forward.

3. Better serialization. Qtile currently doesn't serialize layout state
   across restarts, and it would be nice to have that. It may be possible to
   just pickle and pass the state of each layout, but this would probably also
   involve a re-design of the layout code to share some implementation pieces
   of the window shape(s).