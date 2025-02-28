

Every time you open a software (*component*), this component's window appears on the screen. The window manager controls how this window appears as well as where.
There are **3 main types of Window Managers (WM)**:

- **Stacking WM**
	- Stacking window managers are like papers on a desk, they stack on top of each other, the windows float around freely on the screen. That's the window managers you most often see on desktops like Windows or Mac OS.
- **Tiling WM**
	- Tiling WM arrange each window so that they are all visible at the same time and no window appear on top of each other. They are called that because the window slowly form tiles on the desktop.
	- **Very little mouse usage, all navigation is done using shortcuts**
- **Dynamic WM**
	- Simple enough, they can switch between **Stacking WM** or **Tiling WM** whenever the user needs them to


# i3WM
[Official Page](https://i3wm.org)
**Tiling WM**

Simple and efficient tiling WM with automatic window tiling and dynamic workspaces, driven by keyboard shortcuts.

![[i3_shortcuts.png]]

>[!danger] Designed specifically for [[Display Servers|Xorg]]
## Customizing **i3WM**

On your first launch into **i3**, it'll start the `i3-config-wizard` which will create the initial config file inside `/etc/i3/config`, and also prompt you for your *mod* key (`alt` or `win`) *`alt` for the win ;)*

To get **i3** set up nice, we first off have to copy it's config file into our user directory.
copy it to the `.config` in your home directory
```bash
cp /etc/i3/config ~/.config/i3/config
```
This allows you to have different **configs** for each user, even though, let's face it, you're alone.

>[!info] Anything that **i3** does visually can be configured inside it's `config` file.
>The [official **i3** guide](https://i3wm.org/docs/userguide.html) is really easy to understand and covers pretty much everything tbh.
>*Start by looking at how to remove the borders, then add gaps*

## Custom Commands

The i3 config file let's you run specific commands upon startup.
Just like it's other customization commands, just add them at the end of it's `~/.config/i3/config`.

to execute a specific shell command, you can use `exec <shell-command>` or 
`exec_always <shell-command>`

the `exec` executes the shell command when i3 is **initially** started or reloaded.
the `exec_always` executes the shell command anytime **i3** is started or reloaded.

For instance, setting a background using [feh](https://feh.finalrewind.org):
```bash
echo "exec_always feh --bg-fill /path/to/image.png" >> ~/.config/i3/config
```

this will set the image.png as a background in fill mode every time **i3** is started.

***
# DWM
[Official Page](https://dwm.suckless.org)
**Dynamic WM**

Minimalist WM focusing on simplicity and efficiency, allowing extensive customization through manual source code editing.

Part of the **[Suckless](https://suckless.org)** suite of components, along with [[Terminal Emulators#ST|st]] and **dmenu** *<- still no clue what this is btw*. Which means it's customization also works with [[Patching|patches]]

>[!danger] Designed specifically for [[Display Servers|Xorg]]

***
# Hyprland
[Official Page](https://hyprland.org)
**Tiling WM**

**Hyprland** is a **Tiling Compositor**, which means it acts as a window manager, but also as a [[Compositors|compositor]]. 

>[!danger] Designed specifically for [[Display Servers#Wayland|Wayland]]
# Sway
[Official Page](https://swaywm.org)
**Dynamic WM**

Basically **i3WM** on **Wayland**

>[!danger] Designed specifically for [[Display Servers#Wayland|Wayland]]