---
title: Setting up Fusion Disk on an older Mac
---

Warnings:

- This process is completely destructive of all volumes that are being “fused”. Back up all your data. It WILL be lost.
- The first drive that fails in your Logical Volume Group may cause the entire group to be unusable. Regularly back up all your data. Obviously, Apple has calculated this to be an acceptable risk of the technology.
- Removing your optical drive to install a second hard drive will likely void your MacBook’s warranty.
- This is an advanced topic. If you hose your whole system, I’m not responsible. If you’re afraid of hosing your system, that’s very sensible.

Fusion Drive is the marketing name for a useful application of Core Storage. Core Storage is a [Logical Volume Manager](http://en.wikipedia.org/wiki/Logical_volume_management) for OS X.

To begin, install your physical drives in your Mac. For me, I’m using the 250GB SSD that came with my Early 2011 MacBook Pro 17” along with a 1TB hard disk in the optical bay. In theory, you could use several drives in a Mac Pro. According to an AnandTech article, Apple’s solution only uses 128GB of flash, so this should be plenty sufficient, if not more performant.

Once you’re all built physically, reboot your machine, booting from your Time Machine hard drive. Open a terminal from the Utilities menu. Following Patrick Stein's [examples](http://jollyjinx.tumblr.com/post/34638496292/fusion-drive-on-older-macs-yes-since-apple-has), create your logical volume. Type `diskutil list` to find the identifiers for your disks. I'll repeat myself: This will destroy all data on the disks you specify.

After running the `diskutil cs create` and `diskutil cs createVolume` commands, close the terminal. Your volume has been created. Now, lets get your data back.

For me, restoring from my backup didn’t work for one reason or another (related to nuking the recovery partition). No matter. Select to reinstall OS X. After setup is complete, use Migration Assistant to restore your backup. This isn’t quite like a Time Machine restore, but close enough for my liking.

There is no next step. Enjoy!