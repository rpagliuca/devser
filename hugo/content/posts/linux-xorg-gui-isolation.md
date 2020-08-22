---
title: "GUI Isolation on Linux"
date: 2020-08-22T07:42:36-03:00
---

## Introduction

The problem of GUI isolation on Linux is supposed to be fixed with the upcoming Wayland display manager.

But as of today, many systems still default to using X.Org, so it is still relevant to explore some properties of the X.Org display manager that could be abused security-wise.

## Key concepts

### Display managers

The software that is responsible for talking to hardware (displays/monitors, keyboards, mouse) via boards and drivers, and redirecting their input/output to application graphical windows (clients), like Gnome-Terminal, Mozilla Firefox, Google Chrome, Thunderbird, etc.

### Window managers

The software that gives super-powers to the usually super basic windows provided by the display managers.

### Window compositors

The software that provides nice visual effects for windows.

## Layered architecture

You can have a fully working Linux system without graphics by choosing not to run a display manager and the subsequent layers. More than that, you can choose to stop by whichever layer you prefer, i.e., you can also choose to run your system without a window manager (you will have very basic windows provided by the display manager which cannot be moved or resized), or without a window compositor (windows without opacity control and some other visual tweaks).

## About X.Org design

X.Org has been the most prominent display manager for Linux systems for the past few decades. In conformity with the Linux philosophy, it was designed to give full power to the user of the system, occasionally in detriment of security.

We can understand X.Org by differentiating the following three groups:

1) Hardware: displays/monitors, keyboards, mouse, video cards, USB cards, etc.

2) X.Org Server: a process running on a computer (`ps aux | grep X`) that can send and receive commands/data to the hardware, and redirects the relevant data to X.Org Clients.

3) X.Org Client: GUI processes running on a computer (Gnome-Terminal, Mozilla Firefox, Google Chrome, Spotify, Games, etc.) which connect to the X.Org Server in order to indirectly talk to the hardware.

![](/images/linux-xorg-gui-isolation/xorg-model.png)

## X.Org security concerns (by design!)

First, it's important to reiterate that the following security concerns pertaining to X.Org are not security bugs/flaws per se, but actually intentional design choices that made sense at the time the software was developed.

The security concern: every X.Org Client connected to an X.Org Server can intercept hardware data, even when the GUI application (i.e. X.Org Client) is not on focus.

That means, for example, that it is extremely easy for a keylogger running without special privileges to connect to the X.Org Server and log all the keystrokes on a keyboard, capturing the data and perhaps sending the stolen data through the network.

This aspect has a name: X.Org does not provide GUI isolation – but don't worry! This new display manager called Wayland is supposed to provide it, so keep an eye on that!

## Proof-of-concept

Run the following command on a terminal in a computer running X.Org (currently, most Linux distributions!).

<pre>xinput list | grep -Po 'id=\K\d+(?=.*slave\s*keyboard)' | xargs -P0 -n1 xinput test</pre>

Source: https://unix.stackexchange.com/a/129171/42520

Afterward, start browsing the internet using your favorite web browser and voilà! The terminal running the previous command has listed every keyboard event.

Credit card numbers, passwords, sensitive personal data. They are all there.

## Risks of installing untrusted software

Given the extremely simple and functional proof-of-concept above, you should always weigh the cost-benefits of installing third-party software on your computer.

### Software developers, be wary!

This even goes for software dependencies like npm packages, go modules that you perhaps import when are developing software on your local machine.

It is very easy for this kind of library to run the malicious code in the background when you are writing your own software.

Here are some basic tips:

1) Run your own code inside Docker

2) Never share the X security token to a docker container running untrusted code.

3) Do not run commands as npm start outside Docker

4) Use multi-factor authentication devices (MFA), like Yubikey or Google Authenticator.

## Further reading
http://theinvisiblethings.blogspot.com/2011/04/linux-security-circus-on-gui-isolation.html
