---
layout: post
title: "Linux on a Chromebook with GalliumOS"
tags: 
- unix
---

Are you a mobile programmer interested in a low-cost, lightweight option for hacking on the go? In this post, I describe how to Linux-ify the Samsung Chromebook 3, available now on Amazon for just $200, with GalliumOS, a Linux distribution designed specifically for Chromebooks.

<!-- more -->

## Getting started

Not all Chromebooks are compatible with GalliumOS, [make sure your hardware is supported](https://wiki.galliumos.org/Hardware_Compatibility) before starting. I ordered a [Samsung Chromebook 3](https://www.amazon.com/Samsung-Chromebook-11-6-16GB-XE500C13-K04US/dp/B01N5P6TJW/ref=sr_1_3?ie=UTF8&qid=1527215066&sr=8-3&keywords=samsung+chromebook+3) from Amazon for $200. It had relatively favorable reviews online for the price point and appeared well-supported by the GalliumOS team. Whichever model you choose, record its Hardware ID and Processor from the [Hardware Compatibility](https://wiki.galliumos.org/Hardware_Compatibility) wiki. For example, Samsung Chromebook 3 has ID CELES and an Intel Braswell processor.

There are two primary options for installing GalliumOS onto your Chromebook:
1. Allow your Chromebook to dual-boot into ChromeOS or GalliumOS on system start-up. There is no ability to switch between them without a system restart; if this is a deal breaker for you, you might be interested in [crouton](https://github.com/dnschneid/crouton).
2. Overwrite ChromeOS entirely and only boot your Chromebook into GalliumOS.

This post walks through the steps required for option 1. I imagine I will eventually settle on option 2, but as this is my first Chromebook ever, I want the ability to use ChromeOS from time to time before deciding I can do without it. Since we will not need to wipe ChromeOS, the GalliumOS installation only requires access to the Internet, no external USB drive is necessary. 

## Developer Mode

We must first enable Developer Mode which will allow us to update the firmware (required for most processors) and partition the harddrive for GalliumOS. **Enabling Developer Mode will erase local data on ChromeOS** so back up everything you want to keep (data in the cloud won't be affected).

1. Shut down your device. Boot into Recovery Mode by pressing ESC+F3+Power, where F3 is the Refresh button (clockwise arrow icon).
2. The boot screen will say ChromeOS is damaged or missing. Don't worry, everything is fine.
3. Press CTRL+D to open Developer Mode. Confirm until you see the "OS Verification is OFF" screen and wait for ChromeOS Developer Mode to load. This step will take awhile the first time, around 10-15 minutes.
4. Connect to your WiFi network (required, since we must download a firmware update) and log into the Guest account.
5. You will now see an empty Chrome browser window.

## Firmware update

Depending on your Chromebook model, a [firmware update is required or at least recommended](https://github.com/reynhout/chrx#chromebooks). We will let [MrChromebox.tech](https://mrchromebox.tech/) take care of this for us.

1. From the empty Chrome browser, press CTRL+ALT+T to open the Terminal.
2. Enter `shell` at the prompt.
3. Enter `cd; curl -LO https://mrchromebox.tech/firmware-util.sh && sudo bash firmware-util.sh` to run the MrChromebox Firmware Utility Script (Note:`-LO` contains a capital O, not the number 0).
4. You will be presented with [several firmware update options](https://mrchromebox.tech/#fwscript). Since we are allowing our Chromebook to dual-boot into ChromeOS or GalliumOS, we only need to select the `Install/Update RW_LEGACY Firmware` option.
5. After selecting this option, follow the on-screen instructions to completion. When it's finished, press Q to quit.

## Install GalliumOS

We will use [chrx](https://chrx.org/) (**Chr**omebook Uni**x**) to install GalliumOS on our Chromebook. It can actually install many different Linux distros, but GalliumOS is its default.

1. Make sure you're once again at the shell. (Firmware update steps 1 and 2)
2. Enter `cd ; curl -Os https://chrx.org/go && sh go` and follow the instructions to completion. For what it's worth, I chose to allot 9 GB to GalliumOS (the default).
3. Reboot and repeat Developer Mode steps 3, 4, 5, and Firmware update steps 1, 2.
4. We will now run `chrx` to configure our GalliumOS installation. The following command contains my configuration options, so **don't copy and paste the following command without modifying it**: `cd ; curl -Os https://chrx.org/go && sh go -v -U neal -H voyager -Z America/Los_Angeles -p admin-misc`.
5. This command will:
  - create a system `voyager` with user `neal`, i.e., `neal@voyager`,
  - set the clock to the Pacific Timezone (if you're in the US, Pacific, Mountain, Central, and Eastern are given by `America/Los_Angeles`, `America/Denver`, `America/Chicago`, and `America/New_York`, respectively; see [tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) for a complete international list), and 
  - add the `admin-misc` package, which installs `ssh`, `tmux`, `rsync`, and `vim` immediately after installing GalliumOS; see [packages](https://github.com/reynhout/chrx#packages) for more possibilities. This final step is just a convenience, we could just as well `sudo apt-get install` each one from within GalliumOS once it is up and running.
6. Reboot, and press CTRL+L on the "OS Verification is OFF" screen to boot into GalliumOS. Login with your username and your password (which is the same as your username the first time you log in).

Welcome to GalliumOS! Before you forget, change your password to something more secure: open the Xcfe Terminal from the menu bar and enter `passwd`.

## What's next?

Anything! Well, not _anything_, but you have a fresh Linux install running on your Chromebook which you can configure to your liking. I start off by installing all the programs I rely on, the essentials, and then modifying the look and feel of the OS to fit my preferred aesthetic. The following steps are entirely optional, they're just what I do on a fresh GalliumOS install.

## Install the essentials
- git: version control
- make: build and install programs
- pandoc: universal document converter (e.g., markdown to html, markdown to tex, etc.) 
- gnome-terminal: use GNOME terminal rather than native Xfce terminal
- dconf-cli: Required for gnome-terminal
- wget: download files over HTTP, HTTPS, FTP, and FTPS
- curl: allows exchange of requests between servers ([curl vs. wget](https://unix.stackexchange.com/questions/47434/what-is-the-difference-between-curl-and-wget))
- python: programming!
- ruby: to generate this website
- zsh: an improved shell

{% highlight bash %}
sudo apt-get update
sudo apt-get install -y git make build-essential pandoc wget \
  curl dconf-cli gnome-terminal zsh ruby ruby-dev \
  python2 python2-dev python2-pip python2-venv \
  python3 python3-dev python3-pip python3-venv
{% endhighlight %}

## Jekyll
This website is built with Jekyll.

{% highlight bash %}
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME=$HOME/gems' >> ~/.bashrc
echo 'export PATH=$HOME/gems/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
gem install jekyll bundle jekyll-paginate jekyll-gist jekyll-feed
bundle update jekyll
{% endhighlight %}

## Settings
- Dark mode: Settings > Appearance > Arc-Dark-GalliumOS
- Invert the X- and Y-axis scroll: Settings > Mouse and Touchpad > Reverse scroll direction  
- Speed up the mouse: Settings > Mouse and Touchpad > Mouse speed 5

## Flux
[f.lux](https://github.com/xflux-gui/fluxgui) warms the computer display to ease your eyes.

{% highlight bash %}
sudo add-apt-repository ppa:nathan-renniewaldock/flux
sudo apt-get update
sudo apt-get install fluxgui
{% endhighlight %}
## Color schemes
[Gogh](https://github.com/Mayccoll/Gogh) makes it very easy to customize the GNOME terminal color scheme (does not work on the Xcfe Terminal native to GalliumOS). Run

{% highlight bash %}
wget -O gogh https://git.io/vQgMr && chmod +x gogh && ./gogh && rm gogh
{% endhighlight %}
and [select from over 100 pre-made themes](https://github.com/Mayccoll/Gogh/blob/master/content/themes.md) or [create your own](https://github.com/Mayccoll/Gogh/blob/master/content/howto.md). I'm partial to the Dracula and Freya themes.
