---
title: "How I Dualboot Bodhi on My Acer Spin 5"
description: "A complete guide to installing Bodhi Linux alongside Windows on my Acer Spin 5 laptop. Happy dualbooting!"
date: "2025-08-06"
tags: ["posts","tech-docs"]
params:
  author:
    name: "Mel Meadwell"
draft: true
---

I've been doing a lot of fiddling (the boring, tedious kind)
on my laptop lately.
I've been running Windows 10 on here for a while,
but a couple years ago I had a GNU/Linux dualboot setup that I really liked.
Not long ago my Linux bug came back, and I got to working on reinstalling
some kind of dualboot setup.

I actually got Bodhi Linux installed and working as dualboot with Windows 10,
but then I did something and fucked it all up,
and now we're here, on a mostly-fresh install---
yes, _here_, this thing I'm writing with---
and I'm gonna document every single step that it took to get here.

Blogumentation! Yeah!

# The Sitch(uation)

The Acer Spin 5, which I bought sometime in late 2020,
comes with Windows 10 pre-installed.
Windows may be necessary for UEFI firmware
(i.e., BIOS, which is what people _still_ call UEFI
despite UEFI mostly having replaced BIOS by now)
updates, which Acer distributes as Windows executable (`.exe`) files.

If you're following along with this guide,
make sure your firmware is updated first!
At the time of writing (2025-08-06) the latest was published in 2021,
so you might already be up-to-date!
I haven't tested any previous versions,
but besides, firmware updates are good for security.
Do yourself a favour and get updated!!!!

This is roughly the same state as my system when beginning the first steps.

# First Steps

We'll be starting from a fresh, up-to-date Windows 10 machine.
There are a few things to download and set up before we really get going.


