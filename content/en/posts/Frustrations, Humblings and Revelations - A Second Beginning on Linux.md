---
title: Frustrations, Humblings and Revelations - A Second Beginning on Linux
date: 2025-10-28
draft: false
---
Back in August, I ran through my thought processes on picking a Linux distribution to use on my primary PC. By the end of that lengthy blog post, I had talked about how I was leaning toward [Manjaro](https://manjaro.org/), with [Fedora](https://fedoraproject.org/) as a possible secondary option. Much of my reasoning for leaning toward Manjaro had to do with a blend of features, a middle ground on the rolling release through the Manjaro Stable branch and so forth. Well, I did decide to go for Manjaro as my primary workstation distro after all and, well… what ensued was a series of frustration after frustration that led me to some degree of discouragement and humbling. Let's talk about it.

# Preparation

Part of why I let my Windows 10 system burn down to the wire was that I knew the process of migration would put me down and out for a week **at best**, but it'd likely be closer to two or three weeks. This was because of the fact that, after using roughly the same file structure for my data for decades, there was always a lot of fractured data that made the risk of forgetting to back something up a very real possibility. Plus, up until now, all of my OS upgrades have coincided with major PC hardware upgrades to the point where those previous systems sat with my data for a few years after I had stopped using them, making it plausible that I could recover lost data by accessing those old systems. Not so, this time. This time, I was migrating my existing primary PC to a whole new type of operating system. And while I was confident I had learned enough about Linux to make it my daily driver at this point in time, I wasn't confident that I wouldn't accidentally forget to transfer something over.

Then, there's the fact that I wanted to get myself a new NVMe solid state drive as, previously, my SSDs had all been SATA drives. This actually worked out to my benefit and brought some level of comfort through the issues I'll talk about later. A debacle where the retailer sent me the wrong drive, in the wrong size, in almost no packaging whatsoever (not even an anti-static bag) ended up pushing the migration back by a few more days.

# Starting the Process

After I got that drive issue sorted, and after I had spent multiple passes of going over my backups and all my previous drives for any possible data I might have stored in a weird spot, I took a deep breath and powered down the system. After inserting the thumb drive (I use Ventoy to test and install distros), I went into the UEFI settings, disabled secure boot, ensured my thumb drive was top of the boot order, and I powered on the system. I took a few moments in the live environment to check a few things to make sure Manjaro was a good fit for my system, but then I opened up the installer menu.

The first time I ran through the installer, I chose **btrfs** and tried to enable full disk encryption. That first installation, unfortunately, went wrong. The decryption prompt would get stuck and the boot times were painfully slow. I decided at that point that I would just redo the whole installation process: *better to do that **now** than after I've reinstalled all my applications and ported over my backups*, I thought.

After a second installation, things definitely went better, though I'm not entirely sure why. I did stick with btrfs because of its file integrity features and I doubted it was what was causing the problem. I would've been okay with ext4 but I figured there wasn't much of a downside to using btrfs on a personal system.

# Getting off the Ground

After I booted into the desktop following the installation, I started playing around and installing some of my basic must-have applications. From the [Brave](https://brave.com/) browser, to [Obsidian](https://obsidian.md/), to [Strawberry Music Player](https://www.strawberrymusicplayer.org/), most of the early programs I installed were either available as native packages or as Flatpaks.

That was until I discovered that my cloud syncing application, MEGAsync, was not available on either. The only way I could download it was [through the AUR](https://aur.archlinux.org/packages/megasync), and I had very little experience with the AUR. I had heard of stability and security issues people sometimes encounter by installing AUR packages, so this made me nervous, particularly for something as important as end-to-end encrypted cloud syncing.

Fortunately, I have access to a [Linux community over on MeWe](https://mewe.com/join/linux3), a decent-sized group that has been able to help me figure these things out. Thanks to one of its members, I learned that the AUR is **generally** a good resource, you just have to put a little thought into it by looking at the Github repo, the AUR web page (particularly the PKGBUILD section) and so forth. As such, I built my first AUR package trying to get MEGAsync working… and frankly, it went over way smoother than I expected! Once I figured out how to translate my existing sync structure into something a little more universal (it was very Windows-like before), I had everything syncing the way it was before.

However, it was around this time that I ran into what has been the biggest issue so far.

# Why is it always Networking?

I've been in the slow process of moving to a different place because, currently, my living area is very small and doesn't have room for any real home lab projects. Another issue is that, for the last decade or so, I've been stuck using USB adapters to connect to the internet, as I don't have ethernet access in this home and don't have permission from the homeowner to run ethernet. Until now, that may have been less than optimal, but at least it worked.

The problem is that because Linux doesn't always have the best peripheral support, this also impacts wi-fi adapters. The one I used on Windows wasn't amazing, being an older Netgear adapter without an antenna, but it did easily access my 5GHz WAP and gave me reasonable speeds. The only other adapter I had on hand was a TP-Link adapter with an antenna. Fortunately, that one **did** work fine on Linux.

As it turns out, the drivers for that other adapter (a much older adapter, I might add) weren't added until kernel version 6.14, and Manjaro Stable runs on 6.12 at the time of this writing, as it's the latest LTS kernel version.

Here's where Manjaro saved me: _it makes kernel installation and switching a breeze_. I installed the latest kernel version, 6.17, and sure enough: that older adapter started working… until it didn't.

See, I had also installed an AUR package that included those drivers, which evidently caused conflict, making even the other TP-Link adapter stop working. I was advised to `sudo dmesg` and look at the error messages there, but not much was given that I could troubleshoot by. I just opted for the safer option: I switched back into kernel 6.12, then uninstalled both that AUR driver package and the 6.17 kernel (for the time being). Sure enough, booting back into 6.12 caused the other adapter to work fine again.

I decided to try a kernel upgrade again, reinstalling 6.17 and seeing if that, alone, would support the Netgear adapter. Once again, this worked for me... until it didn't.

Unfortunately for me, I ran into another issue here: that adapter was always plugged into a frontal port, since it lacked an antenna and thus had next to no signal when plugged into the back of my PC, underneath an enclosed wooden desk. Because of that, I've snagged that adapter more than once, which has probably led to gradual weakening of the connections within the port at the very least, and might have done the same to the adapter over time.

This meant that I basically had no choice but to use that other adapter. If I did that, though, I'd have to suffer for painfully low download speeds which would make my experience significantly worse. Once again, the MeWe Linux community was very helpful. One user who had been helping me figure things out had suggested using a USB extension cord and placing the adapter somewhere with a better signal. I decided to take that suggestion and placed an order for an extender (didn't have any on hand). After I experimented to figure out the best angling for the extension cord, I plugged in the TP-Link adapter.

At first, that worked really well... until it didn't. I was getting much better download speeds but, over time, the speeds degraded massively. I decided to try to reinstall kernel 6.17 again so I could experiment with the Netgear adapter. Once again, this initially worked out very well but speeds began degrading. At that point, I decided the best option was to further experiment with the signal strength. I started up a free and open source Android app for analyzing wi-fi signal strength and, with a combo of that app and manipulation of the cable, I eventually found a spot that gave me **acceptable** speeds, at best. It's far from ideal but it's the best option for me at this point in time until I can move to a new place.

# Lessons Learned & Final Thoughts

The first thing I have to say is that this experience has been extremely **humbling**. I've been playing with Linux for almost three years, and have been learning it professionally for about a year now. As part of that, I took an [Arch Linux course](https://www.packtpub.com/en-us/product/getting-started-with-arch-linux-build-from-the-ground-up-9781806119110) (by Dan Mill, same author as the Ubuntu Server course I'm currently working on) and judging by my successes and comprehension with that course, I figured using an Arch-based distro like Manjaro would be relatively easy to grasp. What I didn't expect is to feel like such a total Linux newbie.

At first, this was discouraging: _what if all that learning I previously had undertaking was really nothing compared to what I **need** to know for professional Linux administration and cybersecurity?_ But after two days of being in the thick of troubleshooting things, I've realized something: yes, I definitely **am** still a Linux newbie, but that's totally fine. Using Debian-based distros is inherently a different experience, as those are designed to be reasonably easy and user-friendly. Arch, on the other hand, is about customization, community, geekery and bleeding-edge technology. It's about breaking Linux down and learning how each element of GNU/Linux interacts with each other to give us a full operating system. Manjaro is arguably the most user-friendly way to get into Arch, but it still has the advantages and the challenges of Arch underneath.

I also discovered that, ironically, AI is ridiculously helpful with figuring out problems you're dealing with. These days, the main AI that I use is Perplexity, as I find it to be the closest to a search engine experience and if you know me, you know that I take a very conservative approach to AI usage so a search engine feel would make sense, but it's also very helpful to have a bunch of sources listed to back up what the AI summarizes.

Another bit of irony I discovered is that I remarked in my distro choice post that I "don’t particularly feel the need to use the AUR" because most of what I want is in the native repos or on Flathub. While that may still be true to an extent, having the AUR has already been a **lifesaver** in providing applications and kernel modules I wouldn't necessarily have on a non-Arch distro.

At this point, less than a week after installing Manjaro, I finally consider myself to be comfortable enough with it to use it as my digital "home". While there are still a couple unresolved issues that may or may not require me to set up a Windows dual boot for certain things (hopefully not), I've learned so much, so quickly, and am already loving my time with this distro!

Ultimately, while I may have felt discouragement and despair at first, what I began to learn is that what I'm **really** experiencing is a sort of "second beginning". It reminded me that, even if you think you're advanced, there's no reason to become arrogant or egotistical. There's always more to learn.

And as long as there's more to learn, you should keep going. A wise man once said:

> "It does not matter how slowly you go, as long as you do not stop."
