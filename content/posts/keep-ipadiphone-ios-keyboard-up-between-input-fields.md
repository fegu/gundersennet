---
_edit_last: "1"
_thumbnail_id: "220"
author: wpgundersen
categories:
  - webtech
cover:
  alt: iOS Keypad
  image: /wp-content/uploads/2013/12/ioskeypadup450px.png
date: "2013-12-29T00:47:18+00:00"
guid: http://www.gundersen.net/?p=219
parent_post_id: null
post_id: "219"
tags:
  - ios
  - ios7
  - ipad
  - iphone
  - javascript
  - programming
title: Keep iPad/iPhone iOS keyboard up between input fields
url: /keep-ipadiphone-ios-keyboard-up-between-input-fields/

---
The iPad and iPhone has more than a few quirks web developers need to handle. One of them is being quite insistent on when to show and hide the on-screen keyboard. The default iPad behaviour, in all iOS versions, is to drop the keyboard between input fields, and to deny programmatically showing the keyboard on focus changes. It is only shown when the user taps an input field.

While this is adequate for most situations, it can be really frustrating in cases of several fixed-width input fields in a row.

Here is how to circumvent this behaviour and give your site's user interface a more natural flow. This solution works for wide and short input fields alike, flawlessly moving the user from zipcode to phone number, or from character to character between multiple single-letter input boxes. Compare entering words on [lexical word finder](http://www.lexicalwordfinder.com/) (without this technique) to entering words on this [wordfeud help](http://boardword.com/) site (with this technique).

We rely on two tricks to make this work. First, we keep the user in the same input box all the time, just changing the position and appearance as we move along. Second, to keep the code changes small and localized, we take advantage of the fact that we can change the `id` of an element.

In this way, even as we are moving the same input field along, we are changing it's `id` (and size, location etc as well) — to avoid having to change the existing program logic of the page. The entire time, all input fields will have the correct id's, even though we have been moving the same field along all the time. We also do not introduce any extra fields to put above the real/existing ones (a trick seen in some other solutions).

This has the additional advantages of always leaving the page in a consistent state should the user choose to unfocus and do something else in the middle of filling out the form, as well as being supported in all browsers - avoiding an iOS-only workaround.

The code snippet with a live functioning demo is hosted on [jsFiddle.net](http://jsfiddle.net/fegu/qGFZ6/) and should be fairly self-explanatory. It shows both the regular and iOS-friendly version.

Most of the magic lies in replacing the common step `nextfield.focus();` to move from one input field to the next, with the function `moveon_ipad(fromfield, nextfieldid)` and realising that there is no previous field to blur (i.e. removing any blur-specific code when moving along, although it stays in for the event when the user clicks elsewhere).

In the demo, each field has a different background colour to allow you to see how the field moves along in the iOS-friendly version while they all stay put in the plain version. In practice, the background colours would be the same (or one of the properties the moveon\_ipad function swaps), so the user would not notice any difference.

While the live demo uses jQuery1.9.1, the code is very short and easily adaptable. The demo also uses absolute positioning to ease the movement, but this is not a requirement.
