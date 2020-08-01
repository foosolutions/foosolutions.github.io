---
title: Game Security - Introduction
subtitle: Multi-part series exploring the complexities of Game Security
cover-img: /assets/img/game_banner.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/game_banner.jpg
tags: [gamesec, intro]
---

It's a tried, proven and trusted concept within infosec that information sharing is paramount for organizations and professionals to stay on the cutting edge and defend their software and infrastructure from the latest threats. During my forays into video game security, both professional and personal, I was amazed by the lack of published tools and research in the space. So, I figured I'd try and do my part to fix that.

To be clear, there is a massive community around gaming, and many games have their own ecosystems that form up around them. All of these have a subset community focused on what's refered to as "hacking" or "modding". This community and their sharing, again, are a whole subject unto themselves, and not what I plan to cover.

So this is the first part of a series of posts I plan to write up on the subject of video game security. We will cover setup and operation of a testing environment, and the general attack surface of MMO video games, some common attack and exploit methods, but most importantly how to harden the client and server to defend against them.

The goal here is to enable development teams. I won't be teaching anyone to build an aimbot, or "disable" my tools from being run online with a simple bit. All of the tooling will need to pull information from the source code, and developer, more than likely built into Visual Studio. The end result will allow developers and QA to analyze and manipulate the game client in a development environment similar to common "modder" practices, without taking the risk of running unknown software. Gone will be the days of throwing together and uploading a patch you "hope" will work, to find out weeks later that's not what the hackers were doing.

The planned outline is as follows, which I'll update as this series progresses.

### Game Security

* Concepts, Risks, and Design (../2020-07-18-gamesec-concepts)
* [Virtualization Lab Setup](../2020-07-13-gamesec-virtualization)
* Tools Environment
* Online Game Attack Surface
* File Attacks
* Memory Attacks
* Network Attacks
* Script-Engine Attacks
* Hardware Input/Output Attacks
* Game Server Hardening
* Game Economy Regulation
* Red Team Practices, Tools, and Development

Stay tuned for updates!

