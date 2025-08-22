---
_edit_last: "1"
_thumbnail_id: "120"
author: wpgundersen
categories:
  - windows
cover:
  alt: lang_keyb_change
  image: /wp-content/uploads/2013/06/lang_keyb_change.png
date: "2013-06-14T11:21:03+00:00"
guid: http://www.gundersen.net/?p=119
parent_post_id: null
post_id: "119"
tags:
  - sql-server
  - windows
  - windows-8
title: 'Keyboard language suddenly changing: solved'
url: /keyboard-language-suddenly-changing/

---
For a long time my keyboard input language sometimes changed for no apparent reason. I have two languages installed, and windows seemed to switch between them at will. It happened almost exclusively while using SQL Server Management Studio, and I wrote it off as an annoying bug in that product.

Then, last night, it happened during a presentation. While the consensus of the audience feedback seemed to be that this was a common occurence, one guy suggested it might be because of an unfortunate keyboard shortcut.

Of course! That had to be it.

So, for all of you multi-lingual windows users out there, here is the solution to this annoying "bug".

It turns out that from Windows XP all the way up to and including Windows 8, pressing the left Shift + Alt will change the keyboard language. Just try it right now.

This happens all the time in SQL Server Management Studio because a common operation is to select some lines of SQL (Shift + arrows) then execute it (Alt + x). Going a bit fast what happens is that the shift key is released milliseconds _after_ pressing the alt key. Voilà: your input language just changed.

To get rid of this shortcut, go to Control Panel, Language and click advanced settings.

![lang_keyb_controlpanel](//gundersen.net/wp-content/uploads/2013/06/lang_keyb_controlpanel.png)

Then, click Change language bar hot keys.

![lang_keyb_controlpanel_change](//gundersen.net/wp-content/uploads/2013/06/lang_keyb_controlpanel_change.png)

From here you can manage the keyboard shortcuts. Just set them all to Not Assigned. You can still use the windows key + space, or just click in the language bar, when you want to switch.

{{< figure align="alignnone" width=423 src="//gundersen.net/wp-content/uploads/2013/06/language%5Fkeyboard%5Ffix.png" alt=" Finally, a sigh of relief" caption=" Finally, a sigh of relief" >}}
