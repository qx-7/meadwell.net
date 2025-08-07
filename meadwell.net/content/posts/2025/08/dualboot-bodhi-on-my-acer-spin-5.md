---
title: "How I Dualboot Bodhi on My Acer Spin 5"
description: "A complete guide to installing Bodhi Linux 7 alongside Windows on my Acer Spin 5 laptop. Happy dualbooting!"
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

## The Sitch(uation)

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

## First Steps

We'll be starting from a fresh, up-to-date Windows 10 machine.
There are a few things to download and set up before we really get going.

## Booting Bodhi

### The Linux kernel

Bodhi Linux 7 LTS comes with Linux kernel version 5.15 at time of writing.
Unfortunately, it's kinda old!
Let's update the kernel now, and enable future updates too.
This is not necessary if using Bodhi Linux 7 HWE,
which provides rolling updates for the kernel.

Since I was silly and installed the LTS version, I have to enable updates now.

To enable rolling updates for our kernel, we'll need to use `apt`
to install the HWE kernel package.
Remember to update our package list first!

```bash
sudo apt update
sudo apt search linux-generic-hwe-22.04
```

After a reboot, our system should be up-to-date
with the latest Bodhi HWE kernel!
At time of writing, this package includes the Linux kernel `6.8`.
This got my touchpad working---`5.15` must not be compatible.

If, for whatever reason, the new kernel doesn't work,
simply boot using GRUB's "Advanced options for Bodhi Linux" option
and selecting one of the previous known-working kernels.
Note that `5.15` might break the touchpad!
See the section on [touchpad](#touchpad).

## Configuring Everything

Now that Bodhi boots, it's time to get the system all finished up!
This section will cover all the miscellaneous additional steps I took
after performing the initial clean install and first boot.

### Hardware compatibility

After installing with the 3rd-party device support option enabled,
my laptop is (almost) completely usable!!

#### Touchpad

If using kernel `5.15`, the touchpad won't wanna work in "I2C" mode.
As a temporary workaround, setting the touchpad to "PS2" rather than "I2C"
in the UEFI settings will enable basic functionality.

Another temporary workaround is to add a kernel option
(temporarily, through the GRUB menu before boot)
when booting the kernel.
Press `e` in the GRUB bootloader to edit boot options for this session.
Find the line that includes `ro quiet splash` or similar,
then add `pci=nocrs` as an additional option.
If you bork something, don't panic!--
this is just temporary; simply reboot and it should be good.

I started writing this touchpad section while running kernel `5.15`
and then realized part-way that a kernel update might just fix it,
no workaround necessary.
Yeah, it did, lol.
Just update the kernel.
At time of writing, kernel `6.8.0` supports the touchpad out of the box.
