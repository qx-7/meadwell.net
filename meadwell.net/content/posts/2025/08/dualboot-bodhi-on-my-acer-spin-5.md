---
title: "How I Dualboot Bodhi Linux on My Acer Spin 5"
description: "A complete guide to installing Bodhi Linux 7 alongside Windows 10 on my Acer Spin 5 laptop. Happy dualbooting!"
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
However, since Windows fucking sucks, Microsoft shits, etc,
I obviously couldn't handle having _just_ Windows on here.
I need a system that _doesn't_ just suck battery juice
and phone home to Microshit all the time!
GNU/Linux to the rescue!!
Do note that Windows may be necessary for UEFI
(i.e., BIOS, which is what people _still_ call UEFI
despite UEFI mostly having replaced BIOS in consumer computers by now)
firmware updates, which Acer distributes as Windows executable (`.exe`) files.
I'm not sure what Linux users typically do about this problem...

If you're following along with this guide,
make sure your firmware is updated before we get going!
At the time of writing (2025-08-06) the latest version was published in 2021,
so you might already be up-to-date!
If not, do yourself a favour and get updated!!!!

The system is now in a ready state for the first real steps.

## First Steps

We'll be starting from a fresh, up-to-date Windows 10 machine.
There are a few things to download and set up before we really get going.

### Downloads from the world wide web

In order to boot into Bodhi Linux for the first time to install it,
we need a "live USB" of Bodhi,
which we can boot the system from.
The tool Rufus does that job well!
We'll need two files:
one for the Bodhi Linux image (`.iso`) file,
and another for Rufus.

#### UEFI firmware updates

As mentioned above, please ensure your firmware is up-to-date.
See the manufacturer support page:
[Acer SP314-54N Product Support](https://www.acer.com/us-en/support/product-support/SP314-54N/NX.HQCEB.003/downloads).
Make sure to download the correct firmware for _your_ machine!
The model number is on the bottom.

The latest version (`1.13`) was released in 2021,
so you may already be up-to-date.

#### Bodhi download

Choose a download for Bodhi Linux from the
[official site](https://www.bodhilinux.com/download/).

Before we continue, I'd like to acknowledge how silly it is for me to say,
"download your preferred Bodhi release image."
This is literally half the reason I wanted to try this distro, lmfao.

I chose the standard release,
but I would recommend either HWE or s76 instead.
Standard uses Linux kernel `5.15` for long-term stability,
but newer kernel versions are more hardware-compatible with my newer system.
The touchpad does not work on `5.15`,
but it does on the current HWE version, `6.8`.

The only difference between Bodhi releases is the kernel version
(aside from the AppPack Edition,
which includes the longterm support version `5.15` like standard,
and then adds a bunch of useful pre-installed software).

Installing different versions of the kernel is pretty easy post-install,
so don't get too hung up on the old AppPack kernel
if you'd otherwise choose that release.
Upgrading is easy!
Of course, installing (and uninstalling) one's own software
is also very easy post-install,
so the AppPack Edition could be easily mimicked.

Anyway, make sure to download the `.iso` of your choice.

#### Rufus

Download Rufus from the
[official site](https://rufus.ie/en/).

If you're following along with the same model as I have,
you'll want to download one of the Windows x64 executables.

This tool will make live USBs that we can boot from directly.
In our case, Bodhi 7.

### Installing stuff in Windows

We'll need to install **firmware updates** and **Rufus**.
Go ahead and do that now.

Run the firmware update executable from our downloads.
It may request admin powers, but it's otherwise mostly hands-off.

Rufus is also very easy to install.

### Making a live USB with Rufus and the Bodhi `.iso`

We'll need a blank USB stick for this step,
as Rufus will format the device to create the live USB.
It doesn't need to be particularly high-capacity.
8GB is more than enough.

{{< note "An aside of potential import" >}}
Actually, it's not strictly necessary to use a USB device.
I've had luck with (micro-)SD cards in the past,
but this depends on the particular motherboard firmware.
Some laptops just won't fucking boot from so-and-so device.
USB sticks are usually pretty safe.
{{</ note >}}

Run Rufus.
Select your USB stick from the "Device" dropdown menu.
Now press the button labelled "SELECT" and find the Bodhi `.iso`;
select it.
All the rest of the default options should be good!

Press the "START" button.
Let Rufus do their thang.

When that's all done and the live USB is ready,
shut down the system and set aside the USB stick.
It's UEFI settings time.

### UEFI settings, yay

## Booting Bodhi

### The Linux kernel

Bodhi Linux 7 LTS comes with Linux kernel version `5.15` at time of writing.
Unfortunately, it's kinda old!
Let's update the kernel now, and enable future updates too.
This is not necessary if using Bodhi Linux 7 HWE,
which provides rolling updates for the kernel.

Since I was silly and installed the LTS version, I have to enable updates now.

To enable rolling updates for our kernel, we'll need to use `apt`
to install the HWE kernel package.
Remember to update our package list first!

```bash {style="nordic"}
sudo apt update
sudo apt install linux-generic-hwe-22.04
```

`here is some code too`

After a reboot, our system should be up-to-date
with the latest Bodhi HWE kernel!
At time of writing, this package includes the Linux kernel `6.8`.
This got my touchpad working---`5.15` must not be compatible or something.

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
