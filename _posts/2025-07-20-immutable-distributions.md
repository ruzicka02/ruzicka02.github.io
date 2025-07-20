---
layout: post
title:  "Immutable Systems in an Ever-changing World"
# categories: academic
---

The future of the Linux desktop is in immutable distributions. And in order to
do more with your desktop, you must first be allowed to do less. Or at least
that is what I was told by the Fedora developers, once their atomic desktops
gained some traction. But why should I do so, why should I prohibit myself from
fully controlling the system that I own? And, maybe more importantly, what does
all that even mean?

## Atomic and immutable

The term **immutable**, in the context of operating system distributions,
usually refers to certain parts of the filesystem being set as read-only.
Fedora Atomic goes the other way around, and declares the entire root filesystem
`/` as read-only, except for `/var` and a few other useful symlinks. Most
notably, you cannot modify the `/usr/bin` directory with your system
executables.

Note that the term **read-only** may also be slightly misleading, as the system
directories are overwritten regularly; for example on every system update. What
it means is, *you* cannot write in these directories, this job is restricted
for the OSTree doing the update. You can also **layer** your packages on top via
`rpm-ostree`, but in a very regulated way.

The term **atomic** goes hand in hand with immutability. Essentially, your
operating system is versioned, and updates are just transitions to a new
version. If anything fails, the old version is still there, so you can rollback
and see what went wrong. If you are familiar with Git, imagine that, but for
your OS.

The main advantage of this atomic approach is the **replicability**. When you
download that new system update, there were most likely many users before you,
using the exact same system packages. If any issue is present with the
specific combination of packages with undeclared dependencies, *others find out
first*. Opposed to that, the standard updating procedure, where you essentially
download the latest version of every package and hope for the best, seems like
a hot mess.

Obviously, the idea of layering more stuff on top goes partially against the
replicability idea. Use it too much, and you get back to the point where you are
the only person with that exact system. That is why these systems prefer to use
**containerization** of your user applications instead of layering. This is why
user-friendly containers are the key to success of immutable desktops, and
partly why [Red Hat puts a lot of effort into
it](https://github.com/containers).

## Other immutable distributions

In this article, I will mostly talk about the Fedora atomic desktops, or in my
case Fedora Kinoite. Note that there are different approaches to the same
concepts. For example, [SteamOS](https://store.steampowered.com/steamos) doesn't
mention immutability anywhere on their website. Why would they, if for the vast
majority of its users, "it just works"?

On the other hand, [NixOS](https://nixos.org/) takes all the immutability
concepts, and cranks them up to 11. Why stop at layering packages, when you can
make your entire system *declarative* from script files, using your own
scripting language? While I see the beauty in this approach, it seems too
excessive to me, and I wouldn't have the guts to run this on my primary system.

## How it all works

In the core principle, you have to treat an immutable system slightly different,
than what you might be used to from other systems. It might not be intuitive at
first, but it seems like a sensible change to me. You do not have a single
package manager for *everything* anymore, but your system essentially splits
into two - the immutable core of your OS, and your messy user applications.
If you cannot enforce the solid principle of dependency management and
replicability on your applications, then don't. But the core of your system will
still boot, no matter what happens.

When installing a new application, an important question arises. How do I
install it. More importantly, what is the *best way* to install it? For
graphical applications, you generally want to use Flatpak, as the de-facto
current Linux standard. While coming with a size overhead and certain
limitations, it offers a decent amount of isolation from your operating system.
You still have other alternatives like AppImage available, but unless they are
really necessary, it doesn't make much sense.

Installing CLI programs and applications is a completely different story.
You can either layer them on top of the OS via `rpm-ostree`, install them in a
`toolbx`, or use a completely custom container via Docker or preferrably Podman.
In many cases, you can do all three of these, and determining the best is not
always easy. Luckily, most users will never care.

After overcoming the initial shock of doing things differently, one intuitively
starts asking an important question - how isn't this the default? While new for
Linux, many other operating systems have been doing so for decades. Windows,
Android, macOS and iOS, they all use some form of isolation of the user programs
from the system. However, unlike Linux, these systems are primarily meant for
end users, generally not tech-savvy enough to care. For Linux, desktop usage may
sometimes look more as an afterthought, and while it is also used on desktops
for a relatively long time, it gained the most traction in the past few years.

## How I use it

As said before, for the graphical applications I mostly use Flatpak. It has
a certain learning curve with the directory permissions, which can be slightly
unintuitive at first, in the "why is my file not here" way. However, after
understanding the permissions system and playing with it (Flatpak
Permissions on KDE, under System Settings), one can understand the benefits.
It surely sounds better than giving every app a permission to access all your
files, as is common in the desktop world.

There were various applications, where I resigned on my hopes of running them
via Flatpak, even though available there. These were
[balenaEtcher](https://etcher.balena.io/) and [Raspberry Pi
Imager](https://www.raspberrypi.com/software/), both serving the purpose
of flashing an OS image on a flash drive. It is highly possible that they are
both usable via Flatpak with correct set of permissions given, but I took the
easy way and used the Balena AppImage.

In CLI, I try to use the OS Tree layering as little as possible, for programs
I really need in the OS. They are the following:

- `pip`: Sensible way to use a global Python environment outside of a venv.
- `zsh`: I prefer to use it instead of the default bash.
- `vim-X11`: Using a text editor in toolbox may be a bit tricky, because the
  filesystems are not identical. For example, you cannot use it to edit files
  under `/etc`, because the container has its own `/etc` directory.
- `htop`: While it works just fine to me in the container, it seemed easier
  to layer it. I could probably live without it.
- `zerotier-one`: Network-level programs generally need to work on the core
  operating system.
- `fzf` + `fd-find`: I use them together with the "Oh my zsh!" plugin. Similar
  reason for layering as with `vim`, they would only look in the mounted
  directories.

This list could probably be reduced if I didn't insist on using the terminal
of the core OS that much. To me, it seems like a decent compromise between the
principles of immutability and the ability to use my own computer.

For most of the remaining programs I use, I install them in a single global
toolbox. While this may be a "bad practice" in containerization, I still view
the container as purely disposable, and I maintain my own script that I could
use to replicate it. This default container runs Fedora, but I also have one
Ubuntu container for TeXlive. In my opinion, TeX works strangely on Fedora,
which tries to manage it on its own via `dnf`, but then provides out-of-date
packages compared to pure TeXlive. I also heard various people recommend using
a container for TeXlive installation anyways, even on a mutable distribution.

Lazygit also has a special place in my heart, so I keep it outside of the
toolbox container. As the latest versions often weren't available via dnf,
which used a non-standard COPR repository anyway, I simply downloaded the binary
from Github and placed it in the `~/.local/bin` directory. I generally wouldn't
recommend this for software I want to keep up-to-date, but lazygit can fetch
new updates on its own, so it is a better idea than it seems.

## Is it worth it?

The million dollar question - is all the described hassle worth the benefits?
To be fair, nobody can answer this for you, I only speak for myself. As of now,
it is worth it for me, and therefore I use it on my personal desktop and laptop.
While coming with various complications, I often find the resulting solution
more elegant than any "quick fix" that I would perform on a standard mutable
system. I also particularly enjoy the benefits of safe and rollbackable updates.

It is very possible that all of this is simply "not for you". However, I
encourage you to at least do your own research on the topic, because I believe
it might be here to stay in the world of Linux desktop.
