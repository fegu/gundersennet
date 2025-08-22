---
_edit_last: "1"
_thumbnail_id: "112"
author: wpgundersen
categories:
  - haskell
  - windows
cover:
  alt: Haskell
  image: /wp-content/uploads/2013/04/haskell_logo.png
date: "2013-04-06T13:17:07+00:00"
guid: http://www.gundersen.net/?p=111
parent_post_id: null
post_id: "111"
tags:
  - haskell
  - windows
  - windows-8
title: Haskell on Windows
url: /haskell-on-windows/

---
Haskell is fascinating, but getting started can be a bit rough - especially if you are on Windows. Here are some necessary steps to get you over these initial stumbling blocks when running the [Haskell Platform](http://www.haskell.org "Haskell Platform") (2012.4 or 2014.2) on Windows.

It is quite obvioius that a lot of the users of Haskell are on other platforms, and as a Windows-user you quickly run into some problems not experienced on other platforms. This is doubly frustrating as a newbie because you haven't had time to develop the necessary intuition about where to look for solutions yet. It also certainly does not help that Haskell error messages are known to be misleading.

**Making Networking code work on Windows**

All networking code, both clients and servers, must be wrapped in withSocketsDo. The withSocketsDo construct initializes the winsock library which is a prerequisite for all networking on Windows. This is not required on any other Haskell platform. On all other platforms, withSocketsDo does nothing, and can therefore safely be added to make code run on all platforms.

The most common error message indiciating a missing withSocketsDo is

getAddrInfo: does not exist (error 10093)

Apart from your own code, several great (and widely-used) libraries also suffer from this problem. In this case just try wrapping the main function call. In the popular live diagnostics package [ekg](http://hackage.haskell.org/package/ekg "ekg") by Tibell, this is done by replacing the call to forkServer with withSocketsDo $ forkServer. I have submitted a [patch](https://github.com/tibbe/ekg/issues/12 "patch") to Tibell so expect this problem to be solved in future releases. But a lot of networking libraries remain.

The withSocketsDo function requires

import Network.Socket (withSocketsDo)

**Adding Cygwin to the path**

When installing packages with cabal, you will sometimes encounter the error message

cabal.exe: The package has a './configure' script. This requires a Unix compatibility toolchain such as MinGW+MSYS or Cygwin.

This is easy to solve, you just need to install [CygWin](http://www.cygwin.com/ "CygWin") and then make sure the binaries are in your path. If you install cygwin to c:\\cygwin then the binaries are in c:\\cygwin\\bin. To add c:\\cygwin\\bin to your path temporarily you can use a command prompt and do

set path=%path%;c:\\cygwin\\bin

Or you can go to Control Panel, System, Advanced, Environment Variables, Edit and add it there permanently. While you are editing paths, look for c:\\program files\\haskell\\bin. If it is there, just delete it. The Haskell Plattform installer (at least as of 2014.02) adds this non-existing path by mistake.

CygWin is a great product, have a look at some of the tools it includes and you will quickly find that you need it on all your windows boxes.

**Upgrading Cabal**

Inevitably, you will run cabal update and get the message

Note: there is a new version of cabal-install available.

As the good citizen you are you will probably want to keep things up to date and try

cabal install cabal-install

You will be greeted with a successful install with no error messages. Yet on your next run on cabal update you will get the same error. Double-checking with cabal --version will tell you that even though the upgrade succeeded, you are still running the old version. How nice.

The reason is that the new cabal is installed in your user folder (for example c:\\users\\<username>\\appdata\\roaming\\cabal\\bin), while the original one bundled with the Haskell Platform is located, well, with the platform (rather unexpectedly in C:\\Program Files (x86)\\Haskell Platform\\2012.4.0.0\\lib\\extralibs\\bin). Since your user folder is either not in the path, or in the path, but later than the platform, you are still executing the old cabal.

The solution is to delete or rename the old cabal binary.

**Installing Encoding**

The encoding package is a godsend for anyone working with a variety of character set encodings. The package will not install on Windows unless you set a flag:

cabal install -f-systemencoding (yes, both dashes are correct)

**Replacing tabs with spaces**

If you are starting out with creating your initial projects in an editor, you may get some surprising parsing errors where all seems well. The reason is that Haskell by default is white-space aware, which, by the way, is a great characteristic it shares with Python. The easiest way to make the parsing errors go away is to set your editor to replace tabs with spaces. In [Notepad++](http://notepad-plus-plus.org/ "Notepad++") (free, and more than sufficient for your first few projects), this is done by going to Settings, Languages and selecting "replace by space" under Tab Settings. Surprisingly enough, this is not default in the built-in Haskell language and syntax highlighting settings in Notepad++.
