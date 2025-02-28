
# Kitty Terminal

see [kitty](https://sw.kovidgoyal.net/kitty/)
Seems cool for the `icat` command which let's you display gifs and images into the terminal.
[dude on reddit](https://www.reddit.com/r/unixporn/comments/1b5p2vb/kitty_based_and_gruvpilled/) made something cool with it.

Really easy to customize along with extensive docs. 


# St
**Suckless Terminal**

**ST** is a really lightweight terminal emulator, it's customization works with [[Patching|patches]]

One issue with **ST** is that, if you're not good in C, and no **patch** exists for the feature you're trying to add, chances are you're stuck.

## Important Files

if you installed **ST** from a package manager it's config files should be in `/etc/st/`, otherwise, it will be in the directory where you installed it.

The `config.h` file is the file where all the customization happens, the `config.def.h` is sort of a default backup for `config.h`, so don't touch it.


