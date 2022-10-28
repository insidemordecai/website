---
title: "Fedora Setup Guide"
date: 2022-10-27T23:05:20+03:00
draft: false

categories: ["Linux"]
tags: ["Fedora", "Post Install"]
---

This is how I go about setting up Fedora Workstation, feel free to follow mine tweaking where necessary.

<!--more-->

## DNF Configuration

By default DNF is slow (maybe DNF5 might change that in the future), to fix that I add these flags to speed things up and set yes as the default option for future DNF commands

```sh
echo 'max_parallel_downloads=10' | sudo tee -a /etc/dnf/dnf.conf
echo 'fastestmirror=True' | sudo tee -a /etc/dnf/dnf.conf
echo 'deltarpm=True' | sudo tee -a /etc/dnf/dnf.conf
echo 'keepcache=True' | sudo tee -a /etc/dnf/dnf.conf
echo 'defaultyes=True' | sudo tee -a /etc/dnf/dnf.conf
```

## System Update

```sh
sudo dnf update -y
```

## Enable RPM Fusion

This will give us access to way more apps/softwares that are not available on the standard Fedora repo.

```sh
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf groupupdate core
```

For more information, check out the [RPM Fusion][rpm-fusion] website

## Enable Flathub

Fedora ships with their own version of Flatpak enabled but due to their FOSS policy, we need to add Flathub to use their repository.

```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## Install Media Codecs

After setting up RPM Fusion, you can add these multimedia packages.

```sh
sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo dnf groupupdate sound-and-video
sudo dnf group upgrade --with-optional Multimedia
```

For more information, check out the [RPM Fusion][rpm-fusion] website

## Add Extra Fonts

```sh
sudo dnf install fira-code-fonts 'mozilla-fira*' 'google-roboto*' -y
```

Sometimes Microsoft fonts are needed:

```sh
sudo dnf install -y curl cabextract xorg-x11-font-utils fontconfig
sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
```

And finally adding, [an altenative to proprietary fonts:][fedora-better-fonts]

```sh
sudo dnf copr enable dawid/better_fonts -y
sudo dnf install fontconfig-font-replacements -y
```

I also add these fonts to `~/.local/share/fonts` since I use them in my terminal and other apps:

- [Comic Mono][comicmono]
- [SF Mono Nerd Font][sf-mono-nf]

{{<alert "circle-info">}}
If not satisfied, there is always the option of adding fonts into the fonts directory from Windows
{{</alert>}}

## Install GNOME Tweaks

This allows us to make some customization changes like adding/removing Titlebar buttons, changing themes and fonts among other things. I tend to change the Monospace Text font to a Nerd Font

```sh
sudo dnf install gnome-tweaks
```

In my experience, changing these helps a lot:

- Hinting: Slight
- Antialiasing: Subpixel (this may vary depending on your display setup)

Use [adw-gtk3] as the theme for legacy apps for them to look cohesive other apps using libadwaita.

## Install Apps and Extensions

Apps I use include:

- Browsers: Firefox (flatpak version), Brave Browser
- Coding: VS Code, Android Studio
- Gaming: Steam
- Multimedia: Spotify, VLC
- Terminal Emulator: Alacritty (including [nautilus-open-any-terminal] and [starship])
- Torrent Client: qBittorent
- Utilities: htop, Xtreme Download Manager (only for YouTube and large downloads)
- Others: Discord, Extension Manager, gThumb, Proton VPN, Solaar (for Logitech peripherals)

Then remove the extra apps not needed, for me these include totem (GNOME Videos), Firefox (RPM version), GNOME Terminal

Typically I play around with some extensions or have mixed feelings about some (\*) but these are the extensions I use:

- AppIndicator and KStatusNotifierItem Support
- Blur my Shell \*
- Caffeine
- Clipboard History
- Dash to Dock
- GSConnect
- Night Theme Switcher
- Rounded Window Corners
- Sound Input & Output Device Chooser (should be baked into GNOME 43)
- Status Area Horizontal Spacing
- User Themes \*

{{<alert>}}
I do not recommend installing a lot of extensions as they could slow down your system or worse some could be left unmaintained or lose support across GNOME updates
{{</alert>}}

## Firefox Tweaks

On Firefox, YouTube shows the scrollbars in fullscreen which make it very easy for me to accidentally click on it and suddenly move down the page. To fix this, I add this line to my [uBlock Origin][ublock-origin] filter:

```text
www.youtube.com##ytd-app:style(overflow: hidden !important;)
```

That line disables YouTube's scroll-to-comments "feature" which to me is not as important but used to be to quickly check video publish date.

[Credit: this reddit comment][reddit-comment-firefox]

## Setup Coding Environment and Dotfiles

I usually create a directory in `~` called `development` then setup Flutter and save its SDK in this directory.

Setup Android Studio with this folder in mind as well.

I also always create another directory in `~` called `programming` to store most of my projects.

At this point I clone my [Dotfiles backup][.dotfiles] into `~` and run the script to setup my config files

## Make It Yours

- Change wallpaper
- Schedule night light
- Add online accounts
- Mute system sounds and mic
- Switch to 24h clock format
- Add, reorder and remove items from the dock

Anyway, feel free to reach out.

Cheers ✌️

[Featured Image Credits: u/Drostina on Reddit][feature-source]

<!-- Links - place alphabetically -->

[adw-gtk3]: https://github.com/lassekongo83/adw-gtk3 "An unofficial GTK3 port of libadwaita."
[comicmono]: https://github.com/dtinth/comic-mono-font "A legible monospace font...  the very typeface you’ve been trained to recognize since childhood"
[.dotfiles]: https://github.com/insidemordecai/.dotfiles "My dotfiles backup repository on GitHub"
[feature-source]: https://www.reddit.com/r/Fedora/comments/yawrfu/5120_x_1440_oc_i_present_you_my_simple_fedora/ "r/Fedora post"
[fedora-better-fonts]: https://github.com/silenc3r/fedora-better-fonts "Free substitutions for popular proprietary fonts from Microsoft and Apple operating systems"
[nautilus-open-any-terminal]: https://github.com/Stunkymonkey/nautilus-open-any-terminal "Nautilus plugin to allow opening any terminal"
[reddit-comment-firefox]: https://www.reddit.com/r/firefox/comments/lija24/comment/gph104v/?utm_source=share&utm_medium=web2x&context=3 "comment on r/Firefox"
[rpm-fusion]: https://rpmfusion.org/Configuration "RPM Fusion's Configuration Page"
[sf-mono-nf]: https://github.com/epk/SF-Mono-Nerd-Font "Apple's SF Mono font patched with the Nerd Fonts patcher"
[starship]: https://starship.rs/ "Command line prompt"
[ublock-origin]: https://ublockorigin.com/ "uBlock Origin - Free, open-source ad content blocker."
