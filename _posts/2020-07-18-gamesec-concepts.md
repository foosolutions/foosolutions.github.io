---
layout: post
title: Game Security - Concepts, Risks, Design
subtitle: What, When, Why, How, Who
cover-img: /assets/img/game_banner.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/game_banner.jpg
tags: [gamesec]
---

Part of the [Game Security Series](../2020-07-07-gamesec-intro)

# What are We Talking About?

So, lots of people like to play video games. The past couple month of nearly global quarantine has definitely not hurt those numbers as people looked for outs to their boredom and stir crazy. It sure got me back into things, where before I was only playing what my work laptop could run.

Now, many of these games have things to be earned over time playing the game. Some games allow these earnings to affect your interactions with other players over an internet connection. As with many physical games and competitions, some players are willing, or even eager, to gain whatever advantage they can, unfair, illegal or otherwise. And popular games with large userbases attract "modders" and "hackers" who are more than happy to find and provide exploits to do so.

What a game development organization's does in defense of this, is their core game security program. These applications maintain gigabytes worth of state in running memory, taking user input and communicating it through the net, receiving real-time data back from all connected players, processing and rendering it all beautifully in real-time. These high resource and low latency requirements lead to many of the common weaknesses found in online games, as well as prevent many security industry-standard solutions and approaches to the problems.

What are these weaknesses? Inherently trusting the client is probably the biggest culprit. Resource constraints are another big one. An easy example here being memory overhead maintaining persistent SSL connections with every connected player, a luxury that must be designed and planned for. State management of user sessions is another complex beast, and mismanagement can lead to much more than just hacker problems. Want your player's character to have a persistent inventory? Someone is going to figure out how to double it's contents by doing a backflip in the right spot.

# Why do I care?

Depending on the game design, maybe you don't care if people modify their game client. If your players interact at all online, these modifications may affect more than just the player who installed them. Although not all modifications are malicious, when shared amongst the community, eventually malicious people start to abuse them.

User experiences can be ruined. Whole ecosystems and communities emptied out. In game economies can be ruined. Many users stop paying for in-game microtransactions when they can get the output for cheaper or free. Many games throughout the years have been decimated when hackers were allowed to run rampant, losing their players to competitor titles.

# How do I do something about this?

Securing the game must be striking the perfect balance between a user experience free enough that everyone wants to be a part of it, while hardening the logic and infrastructure enough no one can take advantage of it.

## Build Security into Development

The approach to securing an online video game is not much different from other technology, and must start at an organizational level. You will have to identify your champions of the effort, but it is not just up to them. The goal of building a **secure** game everyone wants to play needs to be the mantra of everyone on every team. It cannot be simply applied as a wrapper, or final stage or round before shipping. Security must be cooked into the game from the beginning.

## Harden the Client and Server

The game client needs to be protected and validated. The game server and logic must be secured, robust, authoritative, and repudiated. Security cannot be a single rule, or control, but involved in each step.

The game client, just like any end-user software, which runs in a less authoritative state than the user. Sadly, due to performance requirements, the usual security mantra of "never trust the front-end" is simply not possible. This is especially true in a game with peer-to-peer traffic. So some logic must be protected and obfuscated however possible.

The server, needs to be set in an authoritative position for player sessions. The server needs to check and authorize transactions, not just blindly accept input from the clients. When a decision is made, all services must adhere to a decision to move, or kick a player.

## Maintain an Anti-Cheat Program

Even if you did all the correct efforts, and spent the right time in the right places, eventually someone is going to figure out something your team didn't think of. Maybe you'll figure out a fix for that one. But eventually there will be more of them working at it than there are you. And then if you're lucky, and your game will grow enough in popularity to attract the professionals.

This is a constant game of cat and mouse, detect and evade. There is never a silver bullet here, and its often just a grind to keep the numbers down.

## Build a Test Lab

Sometimes enough information can be gleaned through tooling, logs and other data sourced from your game clients. But there may be gaps in data, or instances not planned for, which prevents identification or detection of anomalies indicative of an exploit or tool. Sometimes you need to get your hands dirty to figure out what is really going on.

A safe testing lab should be set up for analyzing tools and exploits found in the wild. This should be done on a network segregated from the rest of your organization, as you will essentially be reverse engineering malware. Doing so, your team can learn what these exploits are **actually** doing, enabling your developers to fix the problem at the source.

The next level, is re-building tools in-house to replicate this functionality, allowing your programmers and QA personnel to both safely reproduce exploits, and even explore the attack surface, all within the safe and controlled development environment.

# Video Game Attack Surface

This has touched on various parts of what in the security industry is called the "attack surface" of an online video game. This describes all the different areas something might be targeted with an exploit to gain some advantage.

We dive into this topic in the next post, [Game Security: Online Game Attack Surface](../2020-07-07-gamesec-intro)