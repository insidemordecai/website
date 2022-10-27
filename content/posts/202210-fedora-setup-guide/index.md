---
title: "Fedora Setup Guide"
date: 2022-10-27T23:05:20+03:00
draft: true

tags: ["Linux", "Fedora"]
---

This is how I go about setting up Fedora Workstation, feel free to follow mine tweaking where necessary.
<!--more-->

## DNF Configuration

By default DNF is slow (maybe DNF5 might change that in the future), add these flags to `/etc/dnf/dnf.conf` to speed things up and just for quality of life improvement

```
max_parallel_downloads=10
fastestmirror=True
deltarpm=True
keepcache=True
defaultyes=True
```

## System Update

```
sudo dnf update -y
```

## Enable RPM Fusion

This will give us access to way more apps/softwares that are not available on the standard Fedora repo.

```
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf groupupdate core
```

For more information, check out the [RPM Fusion] website

## Enable Flathub

Fedora ships with Flatpak enabled but we need to add Flathub.

```
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## Install Media Codecs

After setting up RPM Fusion, you can add multimedia packages.

```
sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo dnf groupupdate sound-and-video
sudo dnf group upgrade --with-optional Multimedia
```

For more information, check out the [RPM Fusion] website

## Add Extra Fonts

```
sudo dnf install -y fira-code-fonts 'mozilla-fira*' 'google-roboto*'
```

Sometimes Microsoft fonts are needed:

```
sudo dnf install -y curl cabextract xorg-x11-font-utils fontconfig
sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
```

And finally adding, better looking altenatives of proprietary fonts we may not get (check out [fedora-better-fonts]):

```
sudo dnf copr enable dawid/better_fonts -y
sudo dnf install fontconfig-font-replacements -y
```

I also add these fonts to `~/.local/share/fonts` since I use them in my terminal and other apps:

- [Comic Mono]
- [SF-Mono-Nerd-Font]

## Install GNOME Tweaks

```
sudo dnf install gnome-tweaks
```

This allows us to make some customization changes like adding/removing Titlebar buttons, changing themes and fonts among other things.

In my experience, setting hinting to `Slight` and antialiasing to `Subpixel` helps a lot.

Use [adw-gtk3] as the theme for legacy apps.

## Install Apps and Extensions

Typically I play around with some extensions or have mixed feelings about some (*) but these are the extensions I use include:

- AppIndicator and KStatusNotifierItem Support
- Blur my Shell*
- Caffeine
- Clipboard History
- Dash to Dock
- GSConnect
- Night Theme Switcher
- Rounded Window Corners
- Sound Input & Output Device Chooser
- Status Area Horizontal Spacing
- User Themes*

Apps I use include:

- Browsers: Firefox (flatpak version), Brave Browser
- Coding: VS Code, Android Studio
- Gaming: Steam
- Multimedia: Spotify, VLC
- Terminal Emulator: Alacritty (including [nautilus-open-any-terminal] and [starship])
- Torrent Client: qBittorent
- Utilities: htop, Xtreme Download Manager
- Others: Discord, Extension Manager, gThumb, Proton VPN, Solaar (for Logitech peripherals)

Then remove the extra apps not needed, for me these include totem (GNOME Videos), Firefox (RPM version), GNOME Terminal

## Firefox Changes

I typically change the following in about:config to true to enable hardware acceleration (should help battery life as well):

`media.ffmpeg.vaapi.enabled` <br>
`layers.acceleration.force-enabled`

On Firefox, YouTube shows the scrollbars in fullscreen which make it very easy for me to accidentally click on it and suddenly move down the page. To fix this, I add this line to my uBlock Origin filter [credit: this reddit comment]:

```
www.youtube.com##ytd-app:style(overflow: hidden !important;)
```

That line disables YouTube's scroll-to-comments "feature" which to me is not as important but used to be to quickly check video publish date.

## Setup Coding Environment

I usually create a directory in `~` called `development` then setup Flutter and save its SDK in this directory.

Setup Android Studio with this folder in mind as well.

Create another directory in `~` called `programming` to store most of my projects.

Clone [.dotfiles] into `~` and run script to setup my config files

## Make It Yours

- Schedule night light
- Add online accounts
- Mute system sounds and mic
- Switch to 24h clock format
- Add, reorder and remove items from the dock
- Change wallpaper

<!-- Links - place alphabetically -->

[adw-gtk3]: https://github.com/lassekongo83/adw-gtk3
[comic mono]: https://github.com/dtinth/comic-mono-font
[credit: this reddit comment]:https://www.reddit.com/r/firefox/comments/lija24/comment/gph104v/?utm_source=share&utm_medium=web2x&context=3
[.dotfiles]: https://github.com/insidemordecai/.dotfiles
[fedora-better-fonts]: https://github.com/silenc3r/fedora-better-fonts
[nautilus-open-any-terminal]: https://github.com/Stunkymonkey/nautilus-open-any-terminal
[rpm fusion]: https://rpmfusion.org/Configuration
[sf-mono-nerd-font]: https://github.com/epk/SF-Mono-Nerd-Font
[starship]: https://starship.rs/