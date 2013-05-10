Headless UPnP Renderer
----------------------

I needed a small headless UPnP media renderer for Linux (for small footprint-use
in a Raspberry Pi or CuBox), but there does not seem to be much small stuff
around.

Found this old project [GMediaRender][orig-project] - but it
was incomplete, several basic features missing and the project has been
abandoned several years ago.

So this is a fork of those sources to resurrect this renderer and add the
missing features to be useful (Original sources in [savannah cvs][orig-cvs]).

To distinguish this project from the original one, this is called
[gmrender-resurrect](http://github.com/hzeller/gmrender-resurrect).

Added so far
  * Support to get duration and position of current stream. This allows
    controllers to show a progress bar.
  * Support basic commands (Only `Play` and `Stop` were working before)
     - `Pause`  : Pause current stream.
     - `Seek`   : Seek to a particular position.
  * When current track is finished, transition to state `STOPPED`
    so that the controller sends us the next song (Actively eventing).
  * Support gapless (via SetNextAVTransportURI to play gapless). Looks like
    the next version of BubbleUPnP will send the right action to support it.
  * Volume/Mute control.
  * Compiles with gstreamer-0.10 and gstreamer-1.0
  * ...

Tested the following control points: BubbleUPnP, 2Player, DK Player, eezUPnP;
Please report what other control-points worked for you - and which didn't.

Issues
------

With gapless playing, some troubles came up with the underlying
gstreamer; most notably this results in 'not responding' after a couple of
hours or days of use; this is with gstreamer 0.10 up to 1.0.7. There was a
[leaking thread bug][gst-bug-699794] whose fix will probably show up in
GStreamer 1.0.8 and might fix this issue. There is still another open
[race condition bug][gst-bug-698750] in gstreamer that affects gapless playing
with gstreamer 1.0 though.

Installation
------------
For installation instructions, see [INSTALL.md](./INSTALL.md)

You can reach me via <h.zeller@acm.org>.


[orig-project]: http://gmrender.nongnu.org/
[orig-cvs]:http://cvs.savannah.gnu.org/viewvc/gmrender/?root=gmrender
[gst-bug-698750]: https://bugzilla.gnome.org/show_bug.cgi?id=698750
[gst-bug-699794]: https://bugzilla.gnome.org/show_bug.cgi?id=699794
