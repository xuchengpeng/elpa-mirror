Osm.el is a tile-based map viewer, with a responsive movable and
zoomable display.  The map can be controlled with the keyboard or with
the mouse.  The viewer fetches the map tiles in parallel from tile
servers via the `curl' program.  The package comes with a list of
multiple preconfigured tile servers.  You can bookmark your favorite
locations using regular Emacs bookmarks or create links from Org files
to locations.  Furthermore the package provides commands to measure
distances, search for locations by name and to open and display GPX
tracks.

osm.el requires Emacs 28 and depends on the external `curl' program.
Emacs must be built with libxml, libjansson, librsvg, libjpeg and
libpng support.
