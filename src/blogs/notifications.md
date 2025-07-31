---
layout: 'layouts/blog.html'
tags: blog
date: 2025-05-08
length: 4 Minute
title: Taskbars Suck
---

Since about two years ago, I've used a factory installation of Hyprland in EndeavourOS. This means most parts of my system are customized to my liking with custom tooling, most important of which are my status bar and notifications daemon. 

{% img "/imgs/notifications_notif.gif", "notification gif", 40 %}
{% img "/imgs/notifications_status.png", "desktop status", 40 %}

These widgets were build using Aylur's GTK Shell (AGS), a framework that provides convenient wrappers around system functionality like the notification daemon and `hyprctl`, the Hyprland CLI. The examples above are my proud results from a month of hard work building my first set of custom widgets. There are a variety of panels on the bottom example showing open workspaces, information on the current open window, as well as the current time and date. The top example was my custom notification daemon with painstakingly crafted effects.

As a quick aside, if anyone attempts to tell you that GTK does not suck for building desktop widgets, you have been thoroughly misled. GTK features such a limited set of CSS properties that make animation anything relatively complex or dynamic a huge pain. Understandably the CSS is intended as a gateway to make development with the toolkit more convenient, but when I have previous knowledge of properties and workflows that served me well in the past with CSS, not having those same tools in what feels like a similar environment feels bad. It's a great way of getting the job done quickly, and for the vast majority of people it will be a pleasant experience. However for someone like me who wants a deeply tailored result, GTK remains a painful solution. I'm still glad it exists as I'm not even sure what an alternative would look like that doesn't involve copious amounts of tooling or bloat.

# And then there was silence...

Anyways, sometime during winter 2024, AGS migrated to a new version, called Astal, turning AGS into the CLI used to manage Astral projects. This new port featured GTK-4 and `.tsx` support among other fancy features. I don't remember exactly what I was doing at the time but I do remember I was busy and too lazy to migrate my codebase - thus I used a frozen package of the old AGS library and continued to use that. At some point in the last few months I randomly decided I didn't want to use the old version anymore, and stopped running the old AGS to force myself to migrate. Except... the migration never really happened.

I'm a lazy person, and at some point I just decided to live with the consequences of my actions, resulting in a good few months of a completely informationless desktop. No time, no workspace indicators, no notifications. `http://google.com/?q=time` became my new best friend, and having to constantly listen for Discord's notification sound started to cause anxiety I was always getting a new message from someone.

{% img "/imgs/notifications_bb.jpg", "megamind meme on no notifications?", 30 %}

# Missing Functionality

I love customizing my system and my tools just the way I want. That's why I use Neovim, that's why I use Typst over LaTeX, and that's why I maintain my dotfiles on GitHub. It was surprising to me then, that I actually didn't find myself missing my old notification and status bar setup that much. Don't get me wrong, I hated having to search the time, but I didn't find myself missing my setup, I found myself missing the **functionality**. I missed the time, I missed notifications - but I didn't miss **my** time or **my** notifications.

This got me thinking. Why didn't I miss a system I worked on so much? Even if the design was bad, the amount of work I put in would usually warrant some lingering sentimental feelings. It turns out in this case, my system really was just *that bad*. After a lot of thinking, I was able to boil down my displeasures into failures in two critical desires:

1. If I want something, I just want that and not much else.
2. If I want something, I want it fast.

Status panels? Failure on count (1). Slow animated notifications? Failure on count (2). 

So, what's the solution? Well it's easy: I can fix my ADHD by shortening the time it takes for a notification to realize by removing the slide-in animation and replacing it with a quick fade-in. As for the clutter... this is when I suddenly had a flashback to a moment in the past.

# Notification Dominance

A fair bit ago, I watched a [video](https://www.youtube.com/watch?v=LbG_a3drzNE&ab_channel=Zaney) from a YouTuber who set up his system without any bars or panels, and had all information like time or desktop status sent as a notification. It seemed interesting, but I had my own fancy setup so I didn't pay too much attention at the time. However, as I came out of my reawakening I started to think more about this approach. It seemed to solve all the problems I had. I could get my information when I wanted quickly, at a repeatable and consistent location, and only get that information without having to see anything else (usually).

Thus we arrive at the current day. No status bar. Just notifications. And in fact I discovered over time that all I really care about from the system is time, so what you're seeing below is all the system information I will ever get from a widget in my system. 

{% img "/imgs/notifications_newnotif.gif", "new notification gif", 35 %}

fin.
