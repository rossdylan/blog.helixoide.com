---
layout: post
title: "Converting from ext4 to btrfs on Fedora"
date: 2013-04-09 12:43
comments: true
categories: fedora, btrfs, ext4, luks, btrfs-convert, lvm, selinux
---

Recently I decided to try and convert my laptop from using ext4 to btrfs. The conversion process is pretty simple but since my laptop uses luks encryption and lvm there are a few extra steps.
Here is a step by step walk through of what I had to do to get my laptop converted from ext4 to btrfs on fedora.
First off make sure that your /etc/dracut.conf file has a line for btrfs.

```
    filesystems += "btrfs"
```

Then run this command:

```
    # dracut
```

This will add the btrfs module to your initramfs so you can mount a btrfs filesystem on boot.
Next boot into a fedora 18 live cd. It needs to be a live cd in order for it to have all the tools required for the conversion process.
Since I am using both luks and lvm on my laptop we need to decrypt the lvm partition, and then make it accessible to the live os.

```
    # cryptsetup luksOpen /dev/<partition with lvm> <some name>
    # vgscan
    # vgchange --available y
```

crypsetup is used to decrypt the encrypted partitions, vgscan rescans /dev for lvm parititons and vgchange is used to make those lvm partitions mountable.
Now that we have access to the lvm partitions, it is time to convert them to btrfs

```
    # btrfs-convert /dev/mapper/<volume to convert>
```

This setup will take a decent amount of time depending on how big your filesystems are. My laptop partitions are relatively small and are on an ssd so it only took a minute or so to convert.
So we now have our partitions converted to btrfs, now we need to change a few settings in order to make sure we can boot properly.
First we need to change /etc/fstab to use btrfs instead of ext4. Then we need to make it so selinux can relabel the filesystem on next boot.

```
    # mkdir /mnt/root
    # mount -t btrfs /dev/mapper/<root_lv>
    # touch /mnt/root/.autorelabel
```

In /etc/fstab you need to change this
```
    /dev/mapper/vg_<hostname>-lv_<volume name>  ext4 default   1 1
```

To this

```
    /dev/mapper/vg_<hostname>-lv_<volume name>  btrfs default   1 1
```

Note: The only thing that needs to be changed is the partition type.

To make sure you can boot far enough to have selinux actually relabel the filesystem make sure to set selinux to permissive in /etc/selinux/config
Once all this is complete you can reboot back into your main os. You not boot correctly the first time since there are probably selinux issues,
however after the relabel is complete reboot again and it should work fine.

