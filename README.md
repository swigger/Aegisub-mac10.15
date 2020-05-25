# Aegisub

For binaries and general information [see the homepage](http://www.aegisub.org).

The bug tracker can be found at http://devel.aegisub.org.

Support is available on [the forums](http://forum.aegisub.org) or [on IRC](irc://irc.rizon.net/aegisub).

## Building Aegisub

### OS X

A vaguely recent version of Xcode and the corresponding command-line tools are required.
Nothing older than Xcode 5 has been tested recently, but it is likely that some later versions of Xcode 4 are good enough.

For personal usage, you can use homebrew to install almost all of Aegisub's dependencies:

	brew install autoconf ffmpeg freetype gettext ffms2 fftw fribidi libass m4 luajit icu4c 

boost:
./bootstrap.sh  --prefix=/Volumes/build/DEST/ --with-icu=/usr/local/opt/icu4c/

wxWidgets is located in vendor/wxWidgets, and can be built like so:
./configure --prefix=/Volumes/build/DEST/ --with-macosx-version-min=10.9 --enable-geometry --enable-imaglist --enable-listctrl --enable-stc --without-qt --with-cocoa --with-cxx=11 
make && make install

Aegisub:
export PKG\_CONFIG\_PATH=/Volumes/build/DEST:/usr/local/opt/icu4c/lib/pkgconfig
autoreconf && ./configure --with-wx-prefix=/Volumes/build/DEST
make && make osx-bundle

`autoreconf` should be skipped if you are building from a source tarball rather than `git`.

## Updating Moonscript

From within the Moonscript repository, run `bin/moon bin/splat.moon -l moonscript moonscript/ > bin/moonscript.lua`.
Open the newly created `bin/moonscript.lua`, and within it make the following changes:

1. Prepend the final line of the file, `package.preload["moonscript"]()`, with a `return`, producing `return package.preload["moonscript"]()`.
2. Within the function at `package.preload['moonscript.base']`, remove references to `moon_loader`, `insert_loader`, and `remove_loader`. This means removing their declarations, definitions, and entries in the returned table.
3. Within the function at `package.preload['moonscript']`, remove the line `_with_0.insert_loader()`.

The file is now ready for use, to be placed in `automation/include` within the Aegisub repo.

## License

All files in this repository are licensed under various GPL-compatible BSD-style licenses; see LICENCE and the individual source files for more information.
The official Windows and OS X builds are GPLv2 due to including fftw3.
