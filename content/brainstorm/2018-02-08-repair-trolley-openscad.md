---
author: brainstorm
date: '2018-02-07T13:00:00.000000+00:00'
excerpt: A repair you can print by just writing less than 50 lines of OpenSCAD code
layout: post
modified: '2018-02-07T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2018/02/08/recycling-trolley-with-code
tags:
- repair
title: Recycling a trolley with code
---

So I have to relocate to Australia and my small, beaten trolley (as in 3 out of 4 wheels left) will not cut it. What options do I have?

1. Go to the mall and buy?: High prices for big trolleys.
2. Browse a second hand website.

Trolleys seem to be relatively scarce at that time... until one comes up on the listing **FOR FREE** but with a broken handle mechanism, *sigh*.

FINE, challenge accepted, I'll fix that.

## Unmount the thing

After a quick examination, an inner piece of the handle bar seems broken, after a small 2 screw disassembly, I try to make a poor substitute by bending a spare IKEA piece of metal, unfruitfully:

<img src="/brainstorm/images/2018/02/internal_mechanism.jpg" alt="internal trolley mechanism with a metal bend improvised attempt at fixing it" width="600" height="400">


Focusing on the underlying problem a broken plastic piece:

<img src="/brainstorm/images/2018/02/broken_piece.jpg" alt="handle bar plastic piece" width="600" height="400">

So, can I skip the assignment with a bit of... what do I have around me? Err... Kapton tape?

<img src="/brainstorm/images/2018/02/not_broken_piece.jpg" alt="handle fix attempt kapton" width="600" height="400">

No, today is no day for "and there I fixed it" type of hacks it seems:

<img src="/brainstorm/images/2018/02/broken_kapton.jpg" alt="handle bar kapton broken again" width="600" height="400">

At this point I decide to just create **my first 3d printed part ever** with **null hobby-level CAD design skills** nor experience. 

I only have an old ruler and my computer. Will I be able to fix this?

## Measure the part and write some code

I recommend keeping your screen split in two, one half with [this fantastic OpenSCAD cheatsheet][openscad_cheatsheet] and the other half with my favourite code editor:

{{< gist brainstorm d47c9ba67d99a8c2ed6bf14ab823e6db >}}


I use the detached [OpenSCAD][openscad] viewer with the external editor option and a VSCode-OpenSCAD plugin for the syntax highlighting.

## OpenSCAD renders

And voilà, without knowing how to click around [any of the fancy CAD][onshape] programs [out][fusion360] [there][tinkercad] and **in less than 50 lines** (comment block included), I have my part ready to show off and print, with a personalized [.dxf][dxf_file_format] screaming pineapple logo from another project:

<img src="/brainstorm/images/2018/02/openscad_action.png" alt="openscad render" width="600" height="400">

Now, I have no 3D printer to print this part. Solution? I just do a OpenSCAD-> [Export as `.stl`][stl_file_format] and next tools I need are:

1. A web browser.
2. A credit card.

## 3DPaaS: 3D Printing as a Service

There's plenty out there, I was in a hurry and went for [3d_hubs][3d_hubs]:

> 3d_hubs is your go-to service for ordering custom 3D printed and CNC machined parts online. Upload your files to get instant quotes from local service providers.

Then the following piece showed on my mail shortly after. I went for 100% infill to make the piece robust and [PETG plastic][petg]:

<img src="/brainstorm/images/2018/02/resulting_part.png" alt="resulting 3d printed piece" width="600" height="400">

If you like what you saw, just join on your local hacker/maker space and [repurpose other seemingly useless junk into something great][freecycle_melbourne] ;)

### For instance, if you happen to live in Melbourne and are keen to learn about how I did this and much more such as CNC milling, lasers, plenty of 3D printers, robots, drones, tools, etc... [come visit us at the "Connected Communities Hacker Space" (CCHS)][cchs]

Happy hacking!
 
## Bonus: securing the junkyard supply

I really wish websites still used [RSS][RSS] nowadays, but luckily there are bridge services like [feed43][feed43] which still keep [the good flames alive][aaron_swartz]. With a pretty simple "Global Search Pattern" on that site pointing to [freecycle][freecycle]:

{{< highlight html >}}
<br /> {%}<br />
{%}
<td>{*}
<a href='{%}'>{%}</a>{*}
{%}{*}
<br /> <p class='textCenter'>{*}
<a href='{%}'>{*}
{{< /highlight >}}

One can fetch [interesting reusable items][feed43_rss_freecycle] and [IFTTT it away][ifttt_rss_twitter] on [on twitter][freecycle_melbourne] as they happen.

That way, anyone can contribute on some of the recycling and fixing (common sense?) described here.

If all of the above fails, just bring it to [St Kilda Repair cafe][stkilda_repair_cafe] ;)

Enjoy!

 [openscad]: https://www.openscad.org/
 [openscad_cheatsheet]: https://www.openscad.org/cheatsheet/
 [dxf_file_format]: https://en.wikipedia.org/wiki/AutoCAD_DXF
 [stl_file_format]: https://en.wikipedia.org/wiki/STL_%28file_format%29
 [3d_hubs]: https://3dhubs.refr.cc/87PLN6L
 [petg]: https://all3dp.com/1/petg-filament-3d-printing/
 [cchs]: https://www.hackmelbourne.org/
 [freecycle_melbourne]: https://twitter.com/FreecycleMelb
 [RSS]: https://en.wikipedia.org/wiki/RSS
 [feed43]: https://feed43.com/
 [feed43_rss_freecycle]: https://feed43.com/6514822453153008.xml
 [freecycle_melbourne]: https://twitter.com/freecyclemelb
 [freecycle]: https://www.freecycle.org/
 [ifttt_rss_twitter]: https://ifttt.com/connect/feed/twitter
 [stkilda_repair_cafe]: https://stkildarepaircafe.org.au/
 [aaron_swartz]: https://sv.wikipedia.org/wiki/Aaron_Swartz
 [onshape]: https://onshape.com
 [fusion360]: https://www.autodesk.com/products/fusion-360/overview
 [tinkercad]: https://www.tinkercad.com/
