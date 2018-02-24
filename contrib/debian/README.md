
Debian
====================
This directory contains files used to package exsolutiond/exsolution-qt
for Debian-based Linux systems. If you compile exsolutiond/exsolution-qt yourself, there are some useful files here.

## exsolution: URI support ##


exsolution-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install exsolution-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your exsolutionqt binary to `/usr/bin`
and the `../../share/pixmaps/exsolution128.png` to `/usr/share/pixmaps`

exsolution-qt.protocol (KDE)

