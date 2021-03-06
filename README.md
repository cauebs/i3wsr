i3wsr - i3 workspace renamer
======
[![Build Status](https://travis-ci.org/roosta/i3wsr.svg?branch=master)](https://travis-ci.org/roosta/i3wsr)

`i3wsr` is a small program that uses [I3's](https://i3wm.org/) [IPC Interface](https://i3wm.org/docs/ipc.html)
to change the name of a workspace based on its contents.

## Details

The chosen name for a workspace is a composite of the `WM_CLASS` X11 window
property for each window in a workspace. In action it would look something like this:

![](https://raw.githubusercontent.com/roosta/i3wsr/master/assets/preview.gif)

## Installation
[Rust](https://www.rust-lang.org/en-US/), and [Cargo](http://doc.crates.io/) is
required, and `i3wsr` can be installed using cargo like so:

```sh
cargo install i3wsr
```

Or alternatively, you can build a release binary,

```sh
cargo build --release
```

Then place the built binary, located at `target/release/i3wsr`, somewhere on your `$path`.

### Arch linux
If you're running Arch you can install either [stable](https://aur.archlinux.org/packages/i3wsr/), or [latest](https://aur.archlinux.org/packages/i3wsr-git/) from AUR thanks to reddit user [u/OniTux](https://www.reddit.com/user/OniTux).

## Usage
Just launch the program and it'll listen for events if you are running I3.
Another option is to put something like this in your i3 config

```
exec_always --no-startup-id exec $HOME/.cargo/bin/i3wsr
```

## Configuration

This program depends on numbered workspaces, since we're constantly changing the
workspace name. So your I3 configuration need to reflect this:

```
bindsym $mod+1 workspace number 1
```

If you're like me and don't necessarily bind your workspaces to only numbers, or
you want to keep a part of the name constant you can do like this:

```
bindsym $mod+q workspace number 1:[Q]
```

This way the workspace would look something like this when it gets changed:

```
1:[Q] Emacs|Firefox
```
You can take this a bit further by using a bar that trims the workspace number and be left with only
```
[Q] Emacs|Firefox
```

## Test environment
To run the tests `Xvfb` needs to be installed and run:

```shell
Xvfb :99.0
```
This sets up a headless x server running on DISPLAY :99.0, then some apps needs to be run in this new server:

```shell
env DISPLAY=:99.0 gpick
env DISPLAY=:99.0 i3 -c /etc/i3/config
```

refer to [.travis.yml](https://github.com/roosta/i3wsr/blob/master/.travis.yml) for a CI example

## Attribution
This program would not be possible without
[i3ipc-rs](https://github.com/tmerr/i3ipc-rs), a rust library for controlling
i3-wm through its IPC interface and
[rust-xcb](https://github.com/rtbo/rust-xcb), a set of rust bindings and
wrappers for [XCB](http://xcb.freedesktop.org/).
