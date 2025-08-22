---
_edit_last: "1"
author: wpgundersen
categories:
  - haskell
date: "2015-09-27T20:12:02+00:00"
guid: https://gundersen.net/?p=385
parent_post_id: null
post_id: "385"
tags:
  - 3.5"
  - floppy
  - functional-programming
  - government
title: Functional Floppy Disks - in 2015
url: /functional-floppy-disks-in-2015/

---
Update December 2015: This project has now ended and the floppy era is over - for me at least. The project has been featured quite a bit in the press and blogosphere. Most notably:

[BBC World Service (14min12sek in)](http://www.bbc.co.uk/programmes/p034h09c) [CBC Spark](http://www.cbc.ca/radio/spark/298-shadow-work-surge-pricing-seeing-through-walls-and-more-1.3289284/floppy-disks-just-what-the-doctor-ordered-1.3293207) [Norwegian National Broadcasting (NRK)](http://www.nrk.no/rogaland/pasientlister-pa-disketter-1.11815307) [Dagbladet (national newspaper)](http://www.dagbladet.no/2015/10/02/nyheter/innenriks/fastlegeordningen/helse/teknologi/41286221/)

While mass production of the venerable floppy disk stopped about 2010, some are still needed for applications designed in the nineties. It is actually possible to work with floppy disks on an almost daily basis at work, in 2015. In fact, I do.

{{< figure align="alignnone" width=450 src="/wp-content/uploads/2015/09/2500floppies%5F3point5gigs%5F450px.jpg" alt="2500 floppies, or about 3.5 GB" caption="2500 floppies, or about 3.5 GB" >}}

Norwegian doctors get one 3.5" floppy in the mail from the government (the [Norwegian Directorate of Health](https://helsedirektoratet.no/)) every month. For a long time this applied to all of them, but for about the last decade a [secure online option](https://www.nhn.no/helsenettet/) has been available. Problem is, a lot of doctors love their MSDOS-based electronic journals and haven't switched yet.

The reason floppy disks are used instead of CD-ROMs or flash drives is partly historical and partly economical. The floppy disks are inexpensive; they cost far less than a USB drive and are far less time-consuming to write to than a CD-ROM for this amount of data. Given the historical restriction of delivery by mail, and the data volume being less than 1.44MB, they are the logical choice.

#### Background

Why a floppy a month? In Norway, every citizen selects one doctor to be their go-to doctor for everyday needs. This patient-to-main-doctor directory is maintained by the government. Since patients are free to switch (to any doctor with an available spot) at any time, the government needs to continuously send doctors a list of their main patients. This list, although essentially only a list of names, is classified as health-related information, leading to some restrictions on distribution.

{{< figure align="alignnone" width=450 src="/wp-content/uploads/2015/09/floppytable%5F450px.jpg" alt="1500 discs, about 2.1 GB. Nicely stacked before a visit from the national broadcasting company (NRK)." caption="1500 discs, about 2.1 GB. Nicely stacked before a visit from the national broadcasting company (NRK)." >}}

At work, we took over the distribution from a large american document management company mid-2014, mostly for kicks. We all had fond memories of floppies growing up and it just seemed like a fun thing to do. Since then we have bought tens of thousands of floppies, emptying the stocks of most local online retailers. We are now buying them from the previous company holding the contract. When that stock is empty, or they refuse to sell us any more, we will start buying from [floppydisk.com](http://floppydisk.com). It is what seems to be the sole global supplier with enough in stock.

#### Functional?

To make sure every floppy gets the correct file, and gets sent to the correct doctor, we developed two applications in Haskell. From the [EBCDIC](https://en.wikipedia.org/wiki/EBCDIC) encoded files we receive every month, one application generates labels for envelopes and floppies. A barcode on each label and a barcode reader on every USB floppy reader makes sure everything matches up. The other Haskell application is the one reading the barcodes on the floppy labels and writing the correct file. Thus, the human operator (usually a temp or intern), doesn't even have to touch the keyboard.

The floppy copy workstations are required by the contract to be airgapped (no network). Fitting, as surfing the web and copying floppies belongs to two entirely different eras.

{{< figure align="alignnone" width=450 src="/wp-content/uploads/2015/09/FloppyOperation%5F450px.jpg" alt="A floppy copy workstation in 2015. The phone handset is just there for giggles." caption="A floppy copy workstation in 2015. The phone handset is just there for giggles." >}}

So there it is, Haskell in production for the Norwegian government. Haskell truly can do anything.

Fun fact: while floppies are indestructible, the floppy readers are not. They seem to last a few thousand writes. Luckily, the drives are still readily available online.

#### The end is near

The government seems to finally close down floppy distribution at the beginning of 2016, forcing any remaining doctors over to newer electronic patient journals. The chosen strategy is just to offer paper printouts instead, requiring error-prone rekeying at the doctor's office. The future may be here at last, entering via paper detour.

{{< figure align="alignnone" width=450 src="/wp-content/uploads/2015/09/floppytower.jpg" alt="Art installation 'Resistance to Change' in my office made from empty floppy containers." caption="Art installation 'Resistance to Change' in my office made from empty floppy containers." >}}
