unionfs-fuse-macos
==================

This is a fork from union-fs (https://github.com/rpodgorny/unionfs-fuse) with small changes made to make it build
on MacOS and also this git repo provides MacOS binaries.

The release is built on MacOS using following software
macOS 12.4
Xcode 13.4.1
cmake version 3.23.0

Optionally if ninja is installed it will build with that rather than make.

To build run `./build.sh`
With the above setup this will build without errors and warnings. The result will be in directory `output`.

Version
-------

This branch is forked from version 2.2 of union-fuse.

Binaries
--------

Prebuilt binaries are in `release/unionfs-fuse-macos-2.2.tar.gz`. These are universal2 macOS binaries (intel and arm64)


Original ReadMe
===============

unionfs-fuse
============

This is my effort to create a unionfs filesystem implementation which is way more
flexible than the current in-kernel unionfs solution.

I'm open to patches, suggestions, whatever...

The preferred way is the mailing list at unionfs-fuse@googlegroups.com
or see http://groups.google.com/group/unionfs-fuse.

Why choose this stuff
---------------------

* The filesystem has to be mounted after the roots are mounted when using the standard module. With unionfs-fuse, you can mount the roots later and their contents will appear seamlesly
* You get caching (provided by the underlying FUSE page cache) which speeds things up a lot for free
* Advanced features like copy-on-write and more

Why NOT choose it
-----------------

* Compared to kernel-space solution we need lots of useless context switches which makes kernel-only solution clear speed-winner (well, actually I've made some tests and the hard-drives seem to be the bottleneck so the speed is fine, too)
