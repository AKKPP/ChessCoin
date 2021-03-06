ChessCoin-qt: Qt GUI for ChessCoin 0.32%
========================================

Build instructions
==================

Debian/Ubuntu
-------------

01.
Make sure that the required packages for Qt development of your
distribution are installed, for Debian and Ubuntu these are:

For Qt4:
sudo apt-get install qt4-qmake libqt4-dev build-essential libboost-dev libboost-system-dev libboost-filesystem-dev libboost-program-options-dev libboost-thread-dev libssl-dev

sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install -y libdb4.8-dev libdb4.8++-dev

For Qt5:
sudo apt-get install qt5-default qt5-qmake qtbase5-dev-tools qttools5-dev-tools build-essential libboost-dev libboost-system-dev libboost-filesystem-dev libboost-program-options-dev libboost-thread-dev libssl-dev libdb++-dev

02.
Download: https://github.com/AKKPP/ChessCoin/archive/master.zip

03.
Unpack ChessCoin-master.zip to Home and rename to ChessCoin.

04.
cd ChessCoin
qmake USE_O3=1 RELEASE=1
make


Alternatively, install Qt Creator and open the `chesscoin-qt.pro` file.

An executable named `chesscoin-qt` will be built.

To build chesscoind create empty folder src/obj/zerocoin

cd src/leveldb
chmod 755 *
cd ..
make -f makefile.unix STATIC=1
strip chesscoind


Windows
--------

Windows build instructions:

- Download the `QT Windows SDK`_ and install it. You don't need the Symbian stuff, just the desktop Qt.

- Compile openssl, boost and dbcxx.

- Open the .pro file in QT creator and build as normal (ctrl-B)

.. _`QT Windows SDK`: http://qt-project.org/downloads


Mac OS X
--------

- Download and install the `Qt Mac OS X SDK`_. It is recommended to also install Apple's Xcode with UNIX tools.

- Download and install `MacPorts`_.

- Execute the following commands in a terminal to get the dependencies:

::

	sudo port selfupdate
	sudo port install boost db48

- Open the .pro file in Qt Creator and build as normal (cmd-B)

.. _`Qt Mac OS X SDK`: http://qt-project.org/downloads
.. _`MacPorts`: http://www.macports.org/install.php


Build configuration options
============================

Notification support for recent (k)ubuntu versions
---------------------------------------------------

To see desktop notifications on (k)ubuntu versions starting from 10.04, enable usage of the
FreeDesktop notification interface through DBUS using the following qmake option:

::

    qmake "USE_DBUS=1"

Generation of QR codes
-----------------------

libqrencode may be used to generate QRCode images for payment requests. 
It can be downloaded from http://fukuchi.org/works/qrencode/index.html.en, or installed via your package manager. Pass the USE_QRCODE 
flag to qmake to control this:

+--------------+--------------------------------------------------------------------------+
| USE_QRCODE=0 | (the default) No QRCode support - libarcode not required                 |
+--------------+--------------------------------------------------------------------------+
| USE_QRCODE=1 | QRCode support enabled                                                   |
+--------------+--------------------------------------------------------------------------+


Berkely DB version warning
==========================

A warning for people using the *static binary* version of ChessCoin 0.32% on a Linux/UNIX-ish system (tl;dr: **Berkely DB databases are not forward compatible**).

The static binary version of ChessCoin 0.32% is linked against libdb 5.0 (see also `this Debian issue`_).

Now the nasty thing is that databases from 5.X are not compatible with 4.X.

If the globally installed development package of Berkely DB installed on your system is 5.X, any source you
build yourself will be linked against that. The first time you run with a 5.X version the database will be upgraded,
and 4.X cannot open the new format. This means that you cannot go back to the old statically linked version without
significant hassle!

.. _`this Debian issue`: http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=621425