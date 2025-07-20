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

## How I use it

## Is it worth it?
