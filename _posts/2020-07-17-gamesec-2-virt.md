---
layout: post
<<<<<<< Updated upstream:_posts/2020-07-17-gamesec-2-virt.md
title: Video Game Security: Introduction
subtitle: Multi-part series exploring the complexities of Game Security
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [gamesec, intro]
---


Initially: https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/
=======
title: Game Security - Testing Environment Virtualization
subtitle: Playing the game within the game
cover-img: /assets/img/game_banner.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/game_banner.jpg
tags: [gamesec, qemu, proxmox, vfio]
---

So as I was getting back ito game security, of course I had to do my research. Turns out these days, all the cool kids are hacking their games using virtual machines. Virtualization provides a number of conveinences, and are especially helpful when running "hacker" tools sourced from darker parts of the internet, without potentially infecting the host machine. You can run any number of different operating systems on the same machine, at the same time, and blast away any and all changes when you're done.
>>>>>>> Stashed changes:_posts/2020-07-13-gamesec-2-virt.md

You could run a secondary, "dirty" setup, but running two gaming rigs gets expensive in terms of price, space, heat, monitors, and more. But this might be required depending on your target.

Running a virtual machine essentially allows full control over the emulated hardware of the system, like being able to develop at the "kernel" level without actually running a kernel debugger, which if you've been there, you know can be tedious. Many anti-cheat implementations use machine fingerprints to identify known threat actors using multiple accounts. Emulated hardware ID's can be manipulated as a way to avoid detection or bypass hardware bans. 

Plus virtualization is cool. But onto the fun stuff.

<<<<<<< Updated upstream:_posts/2020-07-17-gamesec-2-virt.md
Raedon Issues, Nvidia boots just fine
echo "options vfio-pci ids=1002:7	31f,1002:ab38" > /etc/modprobe.d/vfio.conf
=======
# Proxmox VE and Qemu
>>>>>>> Stashed changes:_posts/2020-07-13-gamesec-2-virt.md

[Proxmox Virtual Environment](https://pve.proxmox.com/wiki/Main_Page) is an awesome Debian distribution that I use at my day job (and have no affiliation with). To quote the website, `...an open source server virtualization management solution based on QEMU/KVM and LXC.` To put it very simply, it provides a number of simple wrappers around virtual machines, Linux containers (think Docker's great gran-pappy), and the infrastructure required to support them, and a nice web GUI. Can't say enough good things about them, and all for free! They make their money from support contracts, which you might wish you had after we're done here...

So yes, we're going to be running multiple video games, in Linux, in Windows. But wait, doesn't that mean poor performance and crap framerates? Enter VFIO, or Virtual Function Input/Output.

# GPU (PCIe) Passthrough with VFIO

The decription from [kernel.org](https://www.kernel.org/doc/Documentation/vfio.txt) says: `The VFIO driver is an IOMMU/device agnostic framework for exposing direct device access to userspace, in
a secure, IOMMU protected environment. In other words, this allows safe, non-privileged, userspace drivers.` Essentially, it allows the virtual machine to run a driver, which connects to the actual hardware driver, instead of a program emulating one, for less overhead and near bear-metal performance.

But I dare not foray into this alone, but rather stand on the shoulders of giants much wiser. Searching the internet first turned up [this /r/homelab post](https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/). I walked through this great (though a little dated) article with my first attempt, which was fraught with issues. I tried every little tweak I saw on this page, and many others, which would resolve one issue but lead to another. I couldn't get past the "Error 43" reported by the GPU driver in the Windows VM. But the issue was my own, and the [Gigabyte BXi7G3-760](https://www.gigabyte.com/us/Mini-PcBarebone/GB-BXi7G3-760-rev-10#ov), and old "dirty" box I used in the past. Turns out you really can't do this to any machine with an embedded graphics card, which turns out includes any "laptop" cards. So had to circle the wagons and take my main offline. For science!

While debugging the first run, I had stumbled on the official Proxmox wiki page for [PCI Passthrough](https://pve.proxmox.com/wiki/Pci_passthrough). It list out identical steps to most of the reddit post, but skips a number of steps and settings. Combing through posts on their community forum seems to imply that these steps have been removed over time, as Proxmox seems to take care of things like the machine name, and PCI card settings without needing to hit the command line or edit the VM configuration file. Additionally, there's also a [PCIe article](https://pve.proxmox.com/wiki/PCI%28e%29_Passthrough) which is almost, the same, but different... Hard to tell which is the correct one but the first article was more recent at time of writing, and worked for me, for the most part.

There were some additional steps I needed to get this working, but I didn't write them down... But as usual, the first step after accomplishing anything great, is crack open a beer (or celebration of choice). The next step is of course burn it all down and start over again. Thankfully I remembered to take notes that time, which I have below. But first let's build our machine.

UPDATE: Of course, during the course of writing this, I found [another great GPU passthrough walkthrough artice](https://github.com/bryansteiner/gpu-passthrough-tutorial) which led to additional resources linked below. This uses libvirt and virt-manager on top of Debian, so setup is similar, though has some additional steps and tricks that might fix some of my problems mentioned below. Will update again if that's the case.

# Hardware

I had eventually gotten everything working on my main machine, an mITX board with a single GPU. Switching from my modding machine, to my clean gaming one, to my development Debian desktop, all without having to reboot the machine. I could even run these same machines in parallel, though only one could have the GPU and USB hub. And you can only stick so much memory into 2 slots... And what about testing more than one game client and how they interact? So let's go big or go home, and build a new rig!

Because you obviously read all the material linked above, you know the following requirements, but I'll explain them anyway:

  * A hypervisor server running multiple VM's running games is going to need a lot of memory
  * We wanted a machine that could run more than one game client, so the motherboard needs multiple PCIe x16 slots. 
    * PCI x4 slots are good for additional USB hubs to passthrough.
  * The processor and chipset need to support IOMMU interrupt remapping and isolation. 
  * Articles linked earlier advised using different graphics card models in each slot.
  * A lot of cards will need a lot of power.

So, the hardware:

  * Intel i7-10700K
  * MSI MPG Z490
  * G.SKILL Ripjaws V (2 x 32GB) DDR4 3200
  * NVIDIA GTX 1660 SUPER
  * AMD RX 5700 XT ([broken](#amd-rx-issues))
  * NVIDIA RTX 2060
  * CORSAIR HXi Series HX1000i 1000W
  * Crucial MX500 1TB M.2 NVMe
  * WD Blue 3D 1TB M.2 NVMe
  * WD Blue 3D NAND 1TB SSD

No reason for different NVMe drives, just what was laying around at the time.

UPDATE: Circling back to write this up led me to some lists I wish I had read before starting the build. Use the following pages when selecting hardware:
  * [Hardware that supports IOMMU]
  * [Advised hardware packages if starting from scratch]

# Additional Steps

So, as great as the walkthroughs are, it seems everyone has a few extra steps required for every setup.

## No IOMMU Isolation Groups

Turns out that's stored in the OC section of the BIOS


## AMD RX Issues

[drm:amdgpu_pci_remove [amdgpu]] *ERROR* Device removal is currently not supported outside of fbcon

blacklist amdgpu >> /etc/modprobe.d/blacklist.conf

vfio-pci 0000:03:00.1: Refused to change power state, currently in D3

vfio_bar_restore: reset recovery - restoring BARs


VM reboot issue...
https://forum.level1techs.com/t/vega-10-and-12-reset-application/145666

poweroff problem: https://www.reddit.com/r/VFIO/comments/9x11lo/amdgpu_switching_between_host_and_vm/



UPDATE: Seems there's a [known issue] with AMD GPU's