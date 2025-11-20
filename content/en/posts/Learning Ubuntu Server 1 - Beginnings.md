---
title: Learning Ubuntu Server 1 - Beginnings
date: 2025-10-15
draft: false
---
I've gradually purchased more and more courses on Packt for side projects, such as learning Linux or machine learning. Back in August, I worked on one that taught me how to install, configure and use Arch Linux, which was the last major wall that I was facing when it comes to my learning of the basics of the various major Linux distribution types.

Now, I'm moving on to something a bit more practical: **Ubuntu Server Mastery - Real-World Linux Admin Skills** by the same course author, Dan Mill. In this blog post series, "Learning Ubuntu Server", I will be documenting my progress through this course to show what I've learned and what I've reinforced.

# Installation

The beginning of the course covers the basic concepts: what Ubuntu Server is, what it's used for, and what skills it can help you develop. It then progressed toward downloading the distribution's ISO, setting it up in your virtualization software and then going through the installation process.

I've already set up countless virtual machines on my workstation before, so this part was pretty much a breeze. However, there was one key difference between this and the course: in the course, he installs his virtual machine with Hyper-V Manager on Windows, while I installed it on VirtualBox (my preferred VM manager).

Even the installation process wasn't difficult for me, as I've gotten used to utilizing CLIs and TUIs over the past couple years of learning Linux. However, it was good to have something to follow along with and ensure I was installing it correctly.

# Ubuntu Networking Fundamentals

The first topic Dan Mill covers in this course is how to handle networking on Ubuntu Server. He explains the crucial importance of networking with servers. He then moved on to some of the networking tools present on Ubuntu Server.

## Finding the IP

Command #1 was `ip addr`. He explains that the loopback address is mostly used for troubleshooting and that the address we want to look at right now is under `eth0`, though, on my VM, it was listed as `enp0s3` which, upon [a bit of research](https://askubuntu.com/questions/704035/no-eth0-listed-in-ifconfig-a-only-enp0s3-and-lo), is a "[Predictable Network Interface Name](https://www.freedesktop.org/wiki/Software/systemd/PredictableNetworkInterfaceNames/)" for the network card. `eth0` is the default if the hardware can't be determined for one reason or another, whereas `enp0s3` combines `en` for **ethernet**, `p` for **PCI location** (PCI location 0), `s` for **slot number** (slot number 3).

He also points out the `inet` IP address listed under `enp0s3`, of which mine was a Class A IPv4 address with a 24 subnet (255.255.255.0). He also talks about the IPv6 `inet` address present in the `ip addr` command but mentioned that this course primarily focuses on IPv4 instead.

## Information on the Network Interface

Next, he teaches the command `lshw -class network`, ran as superuser, and runs through some of the fields that are listed.

| Field         | Explanation                                                                                                                                                                       |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Description   | Tells the user what the interface is. In this VM, it shows up as "Ethernet interface" but Dan explains that it may shift to "Fiber interface" if you're using a fiber connection. |
| Logical name  | Shows the interface logical name again; once again, in my case, it's `enp0s3`.                                                                                                    |
| Size          | Indicates the bandwidth that was negotiated between this device and the uplink device. In my case, it's 1Gbit/s.                                                                  |
| Configuration | Lists configuration data for the interface. Dan makes note that you can see what driver version is used by the interface.                                                         |

## Configuring Static IP

Dan starts by explaining what a **static IP** is ("a fixed IP address that's manually assigned to a device, such as a server, and it remains constant unless it's manually changed") and compares that to a **dynamic IP** and DHCP. He also explains how the usage of a static IP is crucial for devices such as servers to be accessible by others.

The next step is to make a backup of the **netplan** file, through the following command: `sudo cp /etc/netplan/00-installer-config.yaml 00-installer-config.yaml.copy`, which copies the original file and creates a new one with the `.copy` suffix within the home directory.

However, when I ran that copy command, it failed, telling me that `00-installer-config.yaml` didn't exist. I navigated to the **netplan** folder (`cd /etc/netplan`) to see what, if anything, was in that directory. All I saw was a different YAML file, `50-cloud-init.yaml`. After I did [a bit of research](https://askubuntu.com/a/1436126), I learned that these two files serve the same purpose but the latter is created by **cloud-init**, rather than the installer.

I ran the copy command again, but with the altered names... except, it made the copy in the netplan directory, so I had to move it back to the root directory with `sudo mv 50-cloud-init.yaml.copy /home/tenshi`.

There, all good. Now, *back to the lesson*.

Next, we open the original YAML file with Nano (`sudo nano /etc/netplan/50-cloud-init.yaml`). In this file, we set `dhcp4` to **false**, then add in some extra fields until it looks like this:

```
network:
	ethernets:
		enp0s3:
			dhcp4: false
			addresses:
			 - [chosen IP]
			routes:
			 - to: default
			   via: [another chosen IP]
			nameservers:
			 addresses: [9.9.9.9,1.1.1.1]
	version: 2
			 
```

The addresses will be different according to your own subnet configuration. In the `routes` section, you're setting up the default gateway (router). This is followed by the `nameservers` segment where you place a default and secondary DNS resolver (in my case, I went with Quad9 as the default and Cloudflare as the backup).

After saving the file in Nano, we enter the command `sudo netplan apply` to save the configuration. If you run `ip addr` again, it should display the static IP you've configured. I tested the connection with `ping google.com` and it went through smoothly.

## IP Route Table

To take a look at the IP route table, use the command `ip route show`. At this point, you should only have one: the one you set up in the **netplan** file. This means all traffic will be routed through that IP address. However, if you want to create a static route for a more complex network, use the command `sudo ip route add [target subnet] via [next hop address, probably router]`.

## Static Hostnames for IPs

In this portion of the chapter, we learn that you can manually set up a hostname for IP addresses. For this, we open the **hosts** file via `sudo nano /etc/hosts`.

There will already be a few hostnames in the file. To add a hostname to your router address, for example, you would go to a new line, enter in the router's IP and follow it with a single space and `router`. In other words:

```
127.0.0.1 localhost
127.0.1.1 innersphere

[router IP] router
# The following lines are desirable for IPv6 capable hosts
...
```

This would allow you to run something like `ping router` instead of having to type the router IP every time. This is also useful if you don't have a DNS server set up.

## Further Troubleshooting Commands

To conclude the networking segment, Dan teaches two more commands:

- `tracepath`: similar to `tracert` on Windows, it displays the various hops the connection takes to get to a destination
- `dig`: lists DNS details on a specific domain name

# Updating and Upgrading Ubuntu Server

This portion of the course was easy for me because I already use the CLI to keep my systems up to date (at least, in most cases). Even so, I'll do a quick run of what was covered:

`apt-get update`

This command retrieves the newest package information from the repositories installed on the server. He covers how to access the sources list through `nano /etc/apt/sources.list`. The CLI sources list was the one major thing in this section that I *didn't* already know how to access before this course.

This command makes sure that you have the latest information on package updates, including security patches.

`apt-get upgrade`

This command installs all updates that were revealed through last time the `apt-get update` command was run. This can potentially cause compatibility issues and stability problems, thus you want to try to *test updates in a staging environment* before rolling them out to a live production server.

That being said, when I personally run `update` & `upgrade` commands, I usually drop the `-get` and run this command `sudo apt update && sudo apt upgrade` to get it all finished in one go.

# Setup SSH and SSH Security

To round out this chapter, Dan discusses that SSH (**S**ecure **Sh**ell) allows encrypted, remote access to a system's command line and runs through basic setup and security. First up, he had us ensure OpenSSH Server was installed during the setup phase, but he explains how you can use **apt** to install it if the system doesn't already have it installed.

Next up, he shows us how to access the SSH configuration file at `/etc/ssh/sshd_config`, and shows us how to check the status of the SSH service through **systemctl**: `sudo systemctl status ssh`. If it's not already running, `sudo systemctl start ssh` should get it up and running.

Next, we will cover SSH security. Dan mentions that the first thing you should do when setting up SSH is to switch the port from the default (**port 22**) to something non-standard, but that you want something that's not already being used by other applications. The purpose is to protect against automated attacks that run against SSH configured for the default port 22. For this lesson, he has us switch to **port 2250** by opening the config file with `nano` as a superuser. First, unhash the line that says `Port 22`, then change the **22** to **2250**. Once the changes are saved, you have to restart the SSH server with `sudo systemctl restart ssh`.

Following the port change, the next suggestion he makes is to disable logging in as root over SSH. The reason is to contain potential damage if an attacker manages to SSH into your system. To do this, open the config file with `sudo nano` again and look for the line that says `PermitRootLogin` and change the value to `no`. This will force anyone with SSH access to log in with a user account and use the `sudo` command to do anything with elevated privileges.

Finally, lock down your SSH traffic. This will prevent traffic from being forwarded directly from your router to the system, which could present risk of brute force attacks. In other words, you don't want to leave any sort of remote management ports (such as 22) open to the internet unless you lock it down to source IPs/networks that you trust. The reason for this is because an attacker gaining remote access to your server via SSH can allow them to gain lateral movement within your network to have easier access in attacking other systems on your network.

He also mentions **key-based authentication** which is more secure than password authentication, since it requires both the password and a private key to log in. To do this, you'll need to install software on the SSH server and client to generate a public-private key pair between the two systems. **Multi-factor authentication** is also an option, but for this course, Dan suggests sticking with a basic password login.

Following this advice, Dan talks about using PuTTY to SSH into the virtual machine but, for whatever reason, I simply could not figure out why I couldn't get access. Every time I'd connect (after many tweaks), it simply kept refusing the connection. I'm really not sure why. I can ping internet servers through the virtual machine, so the IPs set up during the networking portion must be correctly configured, but PuTTY simply does not want to connect. It refuses the connection or times out, no matter what I tweak... I double-checked the configuration for the static IP, I made sure my SSH configuration was accurate, I modified **ufw** rules for good measure (even though it was disabled), but it simply refuses to connect. I read up on the issue and found someone mention that the only way they solved this issue was to reinstall Ubuntu Server without including OpenSSH in the installation process, choosing to install it after the distribution is fully installed. Seeing how this is just a virtual machine for practice, I don't plan to use SSH so it's not a problem for now, but this may be problematic for me down the road when I set up my home lab server.

# Final Thoughts

Aside from the final part of the chapter, this one went well. I'm just not sure what went wrong in the SSH part, whether it was something related to router or firewall settings, or what. Networking isn't my strong suit by any means but I'm proficient enough to troubleshoot issues and that's why this has me so stumped: nothing seems to be making any sense whatsoever. Even AI couldn't help me figure it out. This is *exactly why I want to find a mentor or two* to go to when I run into issues like these that I can't just troubleshoot on my own.

As part of the final moments of this chapter of the course, I also did what I normally do on all Linux installations: I installed both **Ranger file manager** and **btop**. The latter is basically just a much more visually-appealing and readable version of the `top` command, whereas the former makes navigating the Linux file system a hundred times easier. In fact, I honestly don't know why anyone would use the classic Linux command line navigation if Ranger is an option. It makes things so much faster and more efficient, without sacrificing any noticeable amount of speed that traditional command line navigation offers. I also installed **neofetch** for good measure, as it's a good way to quickly grab hardware information.

All of that being said, this was a good overview of the early stages of setting up Ubuntu Server. Never in a million years would the 2023 version of me think that I'd ever become comfortable using an OS that is all CLI-driven, yet here we are. It's a reminder:

***Keep learning, keep pushing your boundaries and fight the discouragement. You can do it!***