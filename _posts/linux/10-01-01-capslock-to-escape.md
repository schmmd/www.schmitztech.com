---
layout: default
title: Capslock to Escape
author: Michael Schmitz
category: linux
---

# Remap Capslock to Escape

Caps Lock has always been a useless key. It has a purpose fewer than once a
month yet snares the careless typist daily. With this trick you can remap it to
the Escape key and transform this useless pain into a convenient key for vi!

## Remapping for X-Windows
Put this in some file, such as `.xmodmap`.

```
! Remap CapsLock to escape
remove Lock = Caps_Lock
keysym Caps_Lock = Escape
```

Now you can edit your .`xinitrc` and add:

```
xmodmap .xmodmap
```

## Remapping for the Console

To also remap at the console you need to do something slightly different. Edit
some file, such as `/etc/keymap`, and insert the following test:

```
keycode 58 = Escape
```

Now you can remap Caps Lock manually by the command `loadkeys /etc/keycode`, or
you can put `loadkeys /etc/keycode` into your `/etc/rc.local` to have the
command executed at boot.
