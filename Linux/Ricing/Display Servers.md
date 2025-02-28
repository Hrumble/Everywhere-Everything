
Display server is what handles the graphical input and output of devices. It's basically what allows stuff to be displayed on your screen, and is necessary for any type of graphical interface.

The most widely used and recognized display server in the Unix world is **[Xorg](https://wiki.archlinux.org/title/xorg)**. Most **[[Desktop Environment|DE]]** components will work with **Xorg** as it's what's being used 80% of the time.

>[!warning] Not all [[Desktop Environment|DE]] components will work with all display servers.
>Some are specifically coded for an implementation with **Xorg** for instance.

However, **Xorg** is not the only display server.

# Wayland

**[Wayland](https://wayland.freedesktop.org)** is a slowly rising display server which is getting used more and more, on [r/unixporn](https://www.reddit.com/r/unixporn/) we can see it mostly used to use [[Window Managers#Hyprland|Hyprland]].