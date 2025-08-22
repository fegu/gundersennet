---
_edit_last: "1"
author: wpgundersen
categories:
  - freebsd
  - windows
date: "2013-06-17T19:22:44+00:00"
guid: http://www.gundersen.net/?p=133
parent_post_id: null
post_id: "133"
tags:
  - cache
  - chrome
  - compile
title: 'Solved: Chrome downloading file instead of displaying page'
url: /chrome-downloading-file-instead-of-displaying-page-solved/

---
A lot of developers are experiencing the same problem: while all other browsers display your latest web creation perfectly, Chrome just downloads the script source file - yes, source.

Several [support](http://productforums.google.com/forum/#!topic/chrome/_cA0qdJ4LHI) [threads](https://getsatisfaction.com/tibbo/topics/chrome_downloads_file_instead_of_displaying_web_page) describe the same problem, suggesting everything from security flaws and misconfigured servers to a chrome bug.

![chrome_download](//gundersen.net/wp-content/uploads/2013/06/chrome_download.png)

The answer is simple: you visited your unfinished site in Chrome before it was configured correctly. Chrome correctly just downloaded your front page script file - **and it has cached that result, showing it to you again and again**.

A simple curl invocation will confirm this, just goes to show how useful simple command-line tools can be.

{{< figure align="alignnone" width=450 src="//gundersen.net/wp-content/uploads/2013/06/chrome%5Fcache.png" alt=" Finally..solved!" caption=" Finally..solved!" >}}

The solution, with the cause now found, is equally simple: menu, _settings_, scroll to bottom, click _advanced_, click _clear browsing data_ and select how long a time you have been frustrated: _last day_ usually does it for me. And then click _Clear browsing data_.

At least Chrome has the choice of time period, to avoid having to erase the entire cache. That would both have taken some serious disk activity as well as slowed down browsing for a few days.
