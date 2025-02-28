
Alright if you're here, it means that:
**You forgot how rust works**

don't worry cause so did I right now, that's now I'm keeping track of what I learn while I learn it this time.

>[!info]
>This is entirely taken from the official Rust Documentation
# Installing Rust

First steps first, install rust darling. I'm on windows, but I'll do it for linux too because why not.
To install rust, we first need to install a command line tool called `rustup`, it manages rust versions as well as tools associated with it.

>[!info] 
>There is another way to install rust without `rustup`, but I don't care.
## Installing on Linux

On Linux just enter the following command to download and run the `rustup` installer
```sh
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

The command should output a 
```text
Rust is installed now. Great!
```
And it's as easy as that.

## Installing on Windows

Like all good services on windows it has a simple installer ready, simply go to [this link](https://www.rust-lang.org/tools/install) and follow the instructions on the installer, it'll prompt you do download visual studio and it's tools. (*for C and C++ stuff*) say yes.

## Troubleshooting

To check that you correctly installed Rust, and that everything is working as intended *because I know you that won't be the case*
run
```sh
rustc --version
```

which should output the version number, commit hash, and commit date for the latest stable released version. *Because logically you should have installed the latest stable version*.

This is my output as I'm writing that
```sh
$ rustc 1.80.1 (3f5fd8dd4 2024-08-06)
```

If you don't get a similar output it means two things:
1. Either you messed it up
2. Rust is simply not in your `%PATH%` so add it

# Updating and Uninstalling

To update to the lates stable release, use `rustup`:
```sh
rustup update
```

To uninstall run:
```sh
rustup self uninstall
```


>[!tip] 
>`rustup` comes with an entire copy of the version's documentation, to open it run:
>```sh
>rustup doc
>```

# Next Step

The next step is running your first program to get a first insight into rust, so baby start with the [[Hello World]]
