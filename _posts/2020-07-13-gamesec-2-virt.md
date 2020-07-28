---
layout: post
title: Game Security, Modding Environment Setup
subtitle: Excerpt from Soulshaping by Jeff Brown
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [books, test]
---

Initially: https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/

https://pve.proxmox.com/wiki/Pci_passthrough
https://pve.proxmox.com/wiki/PCI(e)_Passthrough

No IOMMU Groups...
Turns out that's stored in the OC section of the BIOS


Raedon Issues, Nvidia boots just fine
echo "options vfio-pci ids=1002:731f,1002:ab38" > /etc/modprobe.d/vfio.conf



# AMD VM doesn't POST after reboot (with driver update)...

[drm:amdgpu_pci_remove [amdgpu]] *ERROR* Device removal is currently not supported outside of fbcon

blacklist amdgpu >> /etc/modprobe.d/blacklist.conf

update-initramfs -k all -u

vfio-pci 0000:03:00.1: Refused to change power state, currently in D3



vfio_bar_restore: reset recovery - restoring BARs


VM reboot issue...
https://forum.level1techs.com/t/vega-10-and-12-reset-application/145666

poweroff problem: https://www.reddit.com/r/VFIO/comments/9x11lo/amdgpu_switching_between_host_and_vm/

https://github.com/foxlet/macOS-Simple-KVM