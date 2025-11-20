---
title: Navigating Linux Distros - My Personal Journey
date: 2025-08-29
draft: false
---
I, like many others, made the decision to switch to Linux for my primary PC following the [death of Windows 10](https://support.microsoft.com/en-us/windows/windows-10-support-ends-on-october-14-2025-2ca8b313-1946-43d3-b55c-2b95b107f281). I already take a lot of issues with modern Windows as it is, and decided I wasn't willing to use the privacy nightmare that is Windows 11 (excluding one of my side systems, which I'll talk about in future posts). Plus, with my ongoing career change into IT and cybersecurity, I knew Linux was going to be a necessity so I have about three years of experience experimenting with it at this point.

I knew that, thanks to compatibility layers like Proton, Wine and others, I'd almost certainly be fine at a compatibility level. Already, a majority of the tools and software I use are free and open source. The bits of proprietary software I use are almost all present on Linux, and the ones that aren't are possible to run on Wine.

That left me with one crucial decision: **what distro do I pick?**

# First Line of Thought

There are countless Linux distributions, old and new. Some are more similar to others, while others try something different.

When I first started experimenting and learning Linux, I started by using two 'Windows-friendly' distros: **Linux Mint** and **Zorin OS**. Both of these are based on **Ubuntu**, and Ubuntu is based on **Debian**: this meant that my earliest exposure to Linux was with the operations of Debian derivatives. I got used to **apt** and how to operate with it in the command line. Moreover, those 'Windows-friendly' distros are tailor-made for people making the switch, and Mint is a fantastic distro for a "just works" sort of experience.

That should've made the decision easy, no?

No.

See, if I were just using Linux as an alternative, as a means of ditching Windows, it would have been incredibly easy to just stick with Mint and not give it any further thought. The problem that made me reconsider is four-fold.

# Problem 1: Advancing Beyond Familiarity

I'm *not* just using Linux as an alternative. I've said in conversations that Linux has revitalized the childhood wonder I had regarding personal computing (something I plan to write about in the future), and when I embrace the wonder and geekery of something, I want to explore all the facets and nuances that I can.

This means that while I'd be comfortable using Mint because it's familiar and I'm already at a solid proficiency with its package manager, I'd eventually stagnate to a degree and no longer be learning quite as much as I'd like. While Debian derivatives have arguably the largest amount of package support (outside of the AUR), with Flatpak/AppImage, I knew it wasn't quite as big of a deal as it might have been for Linux users years ago.

This is why I first started considering other options. At first, I figured I'd just run with a different Debian or Ubuntu derivative, something like **PopOS** or maybe even **Kubuntu**. After all, I am still an Nvidia user (for now, probably going AMD for my next card) and Ubuntu derivatives tend to have easier overall support for Nvidia users. That, combined with the familiarity, could've made that one of the easier decisions. But again, it comes back around to not *wanting* to rely solely on familiarity.

After reconsidering my focus on Debian/Ubuntu derivatives, I started looking at the other two popular options: **Fedora** and **Arch**, more specifically Arch derivatives.

Fedora initially seemed like a cinch. A rolling release model that wasn't quite as bleeding edge as Arch. *The* distro at the forefront of Linux innovation. A very large userbase with plenty of native package support. Plus, it has a KDE spin, which meant that running Plasma on Fedora wouldn't feel 'off' like it does on Mint. The primary concern that made me reconsider was the recent news that they're thinking about eliminating support for 32-bit packages. Sure, we'll eventually phase out 32-bit support across all devices, much like we did for 16-bit support after Windows NT became the backbone for all Windows versions, but unlike those times, there are still plenty of crucial pieces of software that are 32-bit, as well as many older games. They've since [retracted the proposal](https://www.reddit.com/r/linux_gaming/comments/1lmp0tg/we_won_the_fedora_change_proposal_to_drop_32bit/) but the fact that they considered it is a bit concerning, since it almost indicates a desire to innovate without care for what might break in the process.

That led me to start considering Arch (derivatives). I had tested **Manjaro** early on, partly thanks to the fact that my dad was experimenting with it at that time as well, but **pacman** really confused me at the time. Sure, I could've used the GUI software manager but like I said: I'm not *just* using Linux as an alternative, I'm exploring it as something that I'm thoroughly enjoying, and understanding how to use the CLI package manager is crucial to me (even though I do tend to favor a GUI installer either way).

I also toyed with **Garuda Linux** and **EndeavourOS**. While Endeavour is a fantastic distro for people who want the Arch experience without manual installation, I quickly ruled it out for a few reasons (one major one which I'll touch on in a moment). Garuda, on the other hand, was a real contender... if I could figure out how to properly use pacman. I liked how Garuda had a lot of gamer-friendly tools but doesn't explicitly market itself as a 'gaming distro'. I also really enjoy the look of [their "Dr460nized" custom Plasma layout](https://youtu.be/GG-xDgdjhjU), though that was far from being very important to me considering how customizable Plasma is in general. Garuda defaults to btrfs, which is a pretty awesome feature, but I'm okay with using ext4 or btrfs, personally.

Things really changed for me when I decided to take up an [Arch Linux course on Packt](https://www.packtpub.com/product/getting-started-with-arch-linux-build-from-the-ground-up/9781806119110?_gl=1*1rq0ir0*_gcl_au*NjA2NDY5MzA2LjE3NTY0NDEwOTUuMTEyNDYwMTc1OS4xNzU2NDQxMTA0LjE3NTY0NDExMDQ.*FPAU*NjA5NjQwODc2LjE3NTY0NDEwOTM.). That course demystified the issues I had with Arch and with pacman, teaching me how to install and configure an Arch system with ease. For the first time, I even started considering going with a custom, vanilla Arch installation: something I never would have even entertained before! That course was what ruled Endeavour out for me, simply because I'd rather just build a custom system with vanilla Arch than to just go for 'Arch, built by someone else'.

# Problem 2: Will I Have to Troubleshoot Every Week?

Things were generally see-sawing between Fedora or an Arch derivative. Two rolling release distributions, two different philosophies.

With Fedora, they test package updates for a bit, opposed to the Arch model of immediately releasing updates to everyone. Meanwhile, unlike Arch, Fedora doesn't have that aspect where you never have to update the distro itself. Sure, that would mean that using Fedora would prevent some level of breakage, but that also meant something else: I wouldn't receive kernel updates quite as often, which could be problematic for not receiving the latest drivers. I don't use a whole lot of crazy peripherals, but having the latest drivers for my GPU is definitely something that helps when it comes to gaming.

Meanwhile, the 'never have to upgrade' aspect of Arch was really appealing to me, as I want to settle on one distro for my main PC. I don't mind distro hopping, but not on a primary system as it simply takes too long to restore backups, reconfigure my system (I need to learn how to automate that aspect) and download all the software and games I was playing. Having Arch constantly up-to-date would be helpful in that regard. The problem is, Arch has always been infamous for breakage, especially if you make use of the AUR. I don't particularly feel the need to use the AUR as, so far, software I want can be found in native packages, Flatpak or AppImage. Still, the instability aspect concerns me.

*I can troubleshoot a PC*. I could do quite a bit of troubleshooting even before I began this career change, being a lifelong PC user who has been using PCs since my dad built me a Windows 3.1 PC when I was just about three years old. The fact that I also took two CompTIA A+ courses certainly doesn't hurt. Nonetheless, I don't want to have to constantly troubleshoot issues or worry about when the next breakage will occur.

That brought me back around to something like Manjaro, particularly the Stable branch. The fact that they, like Fedora, test packages for a few weeks means that I'd *likely* have fewer issues in that regard, and I liked that it was a bit closer to the bleeding edge than Fedora without sacrificing too much stability.

# Problem 3: Value of a Monitor

A more recent concern arises from the fact that I was forced to purchase a new primary monitor a couple months ago, as my previous one ended up with an unfixable issue: a vertical line of dead pixels on my screen. I assume something went wrong with the way the electrical signal was carried to the liquid crystals (it'd shine in random colors), but I didn't open and fix it since it was old anyway. I'll be using that as the display for my server in my upcoming home lab.

This new monitor I purchased, the epic [**MSI PRO MP251 E2**](https://youtu.be/QmjJQOjfkFE), was the first monitor I've ever owned that has HDR support. Previously, only my Steam Deck had that capability and I got hooked on it from there. Inevitably, I knew I needed to look into how Linux handles HDR because I wanted to keep the feature if I could.

Upon research, I discovered that at the moment, the only truly *viable* way of handling HDR (outside of Gamescope on SteamOS) is by using KDE Plasma on Wayland. This further pushed me away from the possibility of using Mint, since Mint doesn't have default Plasma support (and, like I mentioned, it feels 'off' if you manually install it) and, more crucially, its Wayland support is still very limited in beta right now.

Now, I *could* get this working on Kubuntu if I want, but that goes back to the problem of wanting to advance beyond familiarity. Meanwhile, Fedora definitely would work for this, considering they've decided to retire X11 and make Wayland the only window manager starting on Fedora 43.

Likewise, something like Manjaro would work. I saw that some people reported issues getting HDR to work on Garuda, however, and getting it to work on vanilla Arch could potentially work but might require quite a bit of extra work and a lot of Arch Wiki reading. I wouldn't necessarily have *problem* with that, except for the fact that I'm already going to be overwhelmed for a week or two trying to port over the backups, reinstall things and tweak the system to my liking... one more thing like that isn't something I particularly want to deal with, so if I can settle on a distro where it doesn't require too much added work to set up, that'd be very welcome.

# Problem 4: Community, Not Corporate

Finally, one of the major reasons I decided to switch to Linux was the free and open source aspect. Modern Windows is an absolute nightmare for privacy, and moving to Linux is objectively an improvement on all fronts in that regard. Literally any distro I choose would be better for my privacy.

However, I'm also not super keen on the idea of going from one corporation's software to another. It's not really a *deal-breaker* but it definitely is something I take into consideration when choosing any software, especially at an operating system level.

Ubuntu itself is corporate. Kubuntu is a community project but directly reliant on a corporate distro. Fedora is similar, being the community variant of Red Hat. Fedora spins like Bazzite or Nobara would be more viable in this regard, since they're community-led just like Linux Mint. PopOS is corporate as well, though I have a lot more respect for System76 than I do for Canonical or Red Hat.

Manjaro is corporate as well, and the company has had its fair share of controversies. Even so, it's based off of Arch, which exemplifies the idea of community-led open source software. Garuda and Endeavour are both community-led Arch projects, which is a major feather in their respective caps.

If I'm going to use a corporate distro, or a distro that's downstream *from* a corporate distro, I want to carefully weigh them by matter of respect, reputation and transparency.

# So, What's Up?

Truthfully, I haven't made a final decision as of this blog post. However, I'm *very* strongly leaning toward Manjaro, with Garuda and Fedora as close runners-up. Manjaro ticks the most boxes for me, and while it's definitely not the *perfect* solution (no such thing), I feel it has the best overall balance of factors for me.

That said, Garuda does also have some very strong factors that make me also consider it. Frankly, the only *real* reason it's not my number 1 choice right now is due to the fact that there's no option akin to Manjaro Stable: the rolling release is effectively no different from vanilla Arch. I do like that Garuda has their "Dr460nized Gaming Edition" that comes with the Zen kernel, but I can also get the Zen kernel on Manjaro due to how it makes kernel switching reasonably easy. In fact, I recently learned that [having multiple kernels installed can save you some headaches](https://www.reddit.com/r/archlinux/comments/1h0xm6l/comment/lz7e4rz/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) in the rare event that a kernel update on Arch breaks the boot process.

And Fedora, even with the issue of the 32-bit debate, that was just a proposal and, like I said, they retracted it. If not for that 32-bit issue and the concern it raised, like Manjaro, it ticks a lot of boxes for me. At that point, one of the only aspects that gives Manjaro a leg up on Fedora is simply that Manjaro's kernel switching is easy and it also has access to the AUR (which, as I said, I don't plan to use much but who knows?).

Ultimately, I just wanted to write this for a few reasons. First off, I wanted to talk you through my thought processes as someone who, despite being 'new-ish' to Linux, is already extremely geeky about it. Second, I also wanted to give some ideas to others who are in a similar boat. I also figured this wasn't a half-bad subject matter to break the ice on my new blog.

And with that, I want to thank you for reading! If you got through all of that, I applaud your patience and appreciate your interest!
