# dotfiles

This readme along with an install script will help you get everything running
in a few minutes. It contains a bunch of configuration for the [tools I
use](https://nickjanetakis.com/blog/the-tools-i-use). I also have a number of
[blog posts and
videos](https://nickjanetakis.com/blog/tag/dev-environment-tips-tricks-and-tutorials)
related to my dev environment.

## 🧬 Who Is This For?

This project is more than a few config files. **In 1 command and ~5 minutes it
can take a new or existing system and install / configure a number of tools
aimed at developers**. It will prompt or warn you if it's doing a destructive
action like overwriting a config file. You can run the idempotent [install
script](./install) multiple times to stay up to date.

There's too many things to list here but here's the highlights:

- Set you up for success with command line tools and workflows
  - Tweak out your shell (zsh)
  - Set up tmux
  - Fully configure Neovim
  - Create SSH / GPG keys if they don't already exist
  - Install modern CLI tools
  - Install programming languages

**It supports Arch Linux, Debian, Ubuntu and macOS. It also supports WSL 2 for
any supported Linux distro.**

*If you don't plan to run the install script that's ok, everything is MIT
licensed. The code is here to look at.*

## 🧾 Documentation

- [Themes](#-themes)
- [Quickly Get Set Up](#-quickly-get-set-up)
- [FAQ](#-faq)
  - [How to personalize these dotfiles?](#how-to-personalize-these-dotfiles)
  - [How to get theme support in your terminal?](#how-to-get-theme-support-in-your-terminal)
  - [How to add custom themes to the set-theme script?](#how-to-add-custom-themes-to-the-set-theme-script)
  - [How to fix Neovim taking a long time to open when inside of WSL?](#how-to-fix-neovim-taking-a-long-time-to-open-when-inside-of-wsl)
  - [Where is the original Vim config?](#where-is-the-original-vim-config)
- [About the Author](#-about-the-author)

## 🎨 Themes

Since these dotfiles are constantly evolving and I tend to reference them in
videos, blog posts and other places I thought it would be a good idea to
include screenshots in 1 spot.

### Tokyonight Moon

![Tokyonight Moon](https://nickjanetakis.com/assets/screenshots/dotfiles-tokyonight-moon-7320a3fb8a76462e64d13bc125a2f284.jpg)

### Gruvbox Dark (Medium)

![Gruvbox Dark Medium](https://nickjanetakis.com/assets/screenshots/dotfiles-gruvbox-dark-medium-cfde05b1e4ccfa0ca9ebf0c089d99a28.jpg)

I prefer using themes that have good contrast ratios and are clear to see in
video recordings. These dotfiles currently support easily switching between
both themes but you can use any theme you'd like.

If you want to see icons you'll need a "nerd font". There's hundreds of them on
<https://www.nerdfonts.com/font-downloads> with previews. I personally use
Inconsolata NF which these dotfiles install for you.

### Setting a theme

These dotfiles include a `set-theme` script that you can run from your terminal
to set your theme to any of the themes listed above. This script takes care of
configuring your terminal, tmux, Neovim, GitUI and FZF in 1 command.

If you don't like the included themes that's no problem. You can use whatever
you want, there's no limitations. You could choose to manually change the
colors or [adjust the set-theme
script](#how-to-add-custom-themes-to-the-set-theme-script) to add a custom
theme.

After installing these dotfiles you can switch themes with:

```sh
# Available themes are: tokyonight-moon and gruvbox-dark-medium
set-theme THEME_NAME
```

When switching themes your terminal and tmux colors will update automatically,
but if you have Neovim already open you'll need to manually close and open it
or run the `SZ` ([source
zsh](https://nickjanetakis.com/blog/running-commands-in-all-tmux-sessions-windows-and-panes))
alias.

*Not all terminals are supported, if yours didn't change then check [this FAQ
item](#how-to-get-theme-support-in-your-terminal).*

## ✨ Quickly Get Set Up

There's an `./install` script you can run to automate installing everything.
That includes installing system packages such as zsh, tmux, Neovim, etc. and
configuring a number of tools in your home directory.

It even handles cloning down this repo. You'll get a chance to pick the clone
location when running the script as well as view and / or change any system
packages that get installed before your system is modified.

### 🌱 On a fresh system?

We're in a catch-22 where this install script will set everything up for you
but to download and run the script to completion a few things need to exist on
your system first.

**It comes down to needing these packages, you can skip this step if you have
them**:

- `curl` to download the install script
- `bash 4+` since the install script uses modern Bash features
  - This is only related to macOS, all supported Linux distros are good to go out of the box

Here's 1 liners you can copy / paste once to meet the above requirements on all
supported platforms:

#### Arch Linux

```sh
# You can run this as root.
pacman -Syu --noconfirm curl
```

#### Debian / Ubuntu

```sh
# You can run this as root.
apt-get update && apt-get install -y curl
```

#### macOS

If you run `bash --version` and it says you're using Bash 3.X please follow
the instructions below:

```sh
# Curl is installed by default but bash needs to be upgraded, we can do that
# by brew installing bash. Once this command completes you can run the install
# script in the same terminal where you ran this command. Before running the
# install script `bash --version` should return a version > 3.X.

# OPTION 1: Using Apple silicon?
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" \
  && eval "$(/opt/homebrew/bin/brew shellenv)" \
  && brew install bash \
  && bash

# OPTION 2: Using an Intel CPU?
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" \
  && eval "$(/usr/local/bin/brew shellenv)" \
  && brew install bash \
  && bash

# The colors will look bad with the default macOS Terminal app. These dotfiles install: https://ghostty.org/
```

### ⚡️ Install script

**You can download and run the install script with this 1 liner:**

```sh
bash <(curl -sS https://raw.githubusercontent.com/nickjj/dotfiles/master/install)
```

*If you're not comfortable blindly running a script on the internet, that's no
problem. You can view the [install script](./install) to see exactly what it
does. The bottom of the file is a good place to start. Sudo is only used to
install system packages. Alternatively you can look around this repo and
reference the config files directly without using any script.*

*Please understand if you run this script on your existing system and hit yes
to some of the prompts your config files will get overwritten. Always have good
backups!*

**You can also run the script without installing system packages:**

```sh
bash <(curl -sS https://raw.githubusercontent.com/nickjj/dotfiles/master/install) --skip-system-packages
```

The above can be useful if you're using an unsupported distro of Linux in which
case you'll need to install the [dependent system packages](./install) on your
own beforehand. Besides that, everything else is supported since it's only
dealing with files in your home directory.

This set up targets zsh 5.0+, tmux 3.1+ and Neovim v0.10+. As long as you can
meet those requirements you're good to go. The install script will take care
of installing these for you unless you've skipped system packages.

🐳 **Try it in Docker without modifying your system:**

```sh
# Start a Debian container, we're passing in an env var to be explicit we're in Docker.
docker container run --rm -it -e "IN_CONTAINER=1" -v "${PWD}:/app" -w /app debian:stable-slim bash

# Copy / paste all 3 lines into the container's prompt and run it.
#
# Since we can't open a new terminal in a container we'll need to manually
# launch zsh and source a few files. That's what the last line is doing.
apt-get update && apt-get install -y curl \
  && bash <(curl -sS https://raw.githubusercontent.com/nickjj/dotfiles/master/install) \
  && zsh -c ". ~/.config/zsh/.zprofile && . ~/.config/zsh/.zshrc; zsh -i"
```

*Keep in mind with the Docker set up, unless your terminal is already
configured to use Tokyonight Moon then the colors may look off. That's because
your local terminal's config will not get automatically updated.*

**🚀 Keeping things up to date and tinkering**

If you've run the install script at least once you can `cd "${DOTFILES_PATH}"`
and run `./install`. That will pull in the latest remote changes and re-run
things. I suggest first running `./install --diff` or `./install --changelog`
to compare what you have locally to what's [available
remotely](https://github.com/nickjj/dotfiles/commits/master/). Neither command
modifies your git tree.

There's also `./install --update` which will only pull in the latest remote
changes but exit out early before any other actions occur. This could be handy
to pull in and review any changes before they run against your system.

You can also run `LOCAL=1 ./install` to re-run the install script without
pulling updates from this repo. That can be handy for testing your changes
locally or to prevent new updates from being pulled.

### 🛠 Make it your own

If you just ran the install script and haven't done so already please close
your terminal and open a new one.

There's a few ways to customize these dotfiles ranging from forking this repo
to customizing [install-config](./install-config.example) which is git ignored.
The second option lets you adjust which packages and programming languages get
installed.

Before you start customizing other files, please take a look at the
[personalization question in the FAQ](#how-to-personalize-these-dotfiles).

### 🪟 Extra WSL 2 steps

In addition to the Linux side of things, there's a few config files that I have
in various directories of this dotfiles repo. These have long Windows paths and
are in the `c/` directory.

It would be expected that you copy those over to your system while replacing
"Nick" with your Windows user name if you want to use those things, such as my
Microsoft Terminal `settings.json` file and others. Some of the paths and guids
(WSL distros, etc.) may also contain unique IDs too, so adjust them as needed
on your end.

It's expected you're running WSL 2 with WSLg support to get clipboard sharing
to work between Windows and WSL 2. You can run `wsl.exe --version` from WSL 2
to check if WSLg is listed. Chances are you have it since it has been supported
since 2022! All of this should "just work". If clipboard sharing isn't working,
check your `.wslconfig` file in your Windows user's directory and make sure
`guiApplications=false` isn't set.

*If you see `^M` characters when pasting into Neovim, that's a Windows line
ending. That's because WSLg's clipboard feature doesn't seem to handle this
automatically. If you paste with `CTRL+SHIFT+v` instead of `p` it'll be ok. I
guess the Microsoft Terminal does extra processing to fix it for you.*

Pay very close attention to the `c/Users/Nick/.wslconfig` file because it has
values in there that you will very likely want to change before using it.
[This commit
message](https://github.com/nickjj/dotfiles/commit/d0f1fc2622204b809cf7fcbb1a82d45b451064c4)
goes into the details.

Also, you should reboot or from PowerShell run `wsl --shutdown` and then
re-open your WSL instance to activate your `/etc/wsl.conf` file (the install
script created this). That will be necessary if you want to access your mounted
drives at `/c` or `/d` instead of `/mnt/c` or `/mnt/d`.

You may have noticed I don't enable systemd within WSL 2. That is on purpose.
I've found it delays opening WSL 2 by ~10-15 seconds and also any systemd
services were delayed from starting by ~2 minutes.

## 🔍 FAQ

### How to personalize these dotfiles?

The [install-config](./install-config.example) lets you customize a few things
but chances are you'll want to personalize more than what's there, such as
various Neovim settings. Since this is a git repo you can always do a `git
pull` to get the most up to date copy of these dotfiles, but then you may find
yourself clobbering over your own personal changes.

Since we're using git here, we have a few reasonable options.

For example, from within this dotfiles git repo you can run `git checkout -b
personalized` and now you are free to make whatever changes that you want on
your custom branch.  When it comes time to pull down future updates you can run
a `git pull origin master` and then `git rebase master` to integrate any
updates into your branch.

Another option is to fork this repo and use that, then periodically pull and
merge updates. It's really up to you. By default these dotfiles will add an
`upstream` git remote that points to this repo.

### How to get theme support in your terminal?

The `set-theme` script tries to be pretty flexible but it's not super tuned to
support every terminal in every operating system. It will attempt to configure
a number of terminals based on looking for specific config paths.

#### Ghostty

Everything should work out of the box. This gets installed by default on Linux
and macOS.

#### Windows Terminal

These dotfiles have my exact config in
`c/Users/Nick/AppData/Local/Packages/Microsoft.WindowsTerminal_8wekyb3d8bbwe/LocalState/settings.json`,
you can copy / paste that config to a similar path within your Windows set up.
It comes set up with all supported themes. This is only necessary to do once.
It's not automated because it would be pretty rude if the install script
overwrote your Windows files.

Once you have that set up, running `set-theme THEME_NAME` will work without
further manual adjustments.

#### Everything else

If you're using a popular terminal and want it officially supported please open
a pull request. You'd modify `set-theme` for the `TERMINALS` and `THEMES`
dictionaries. The basic idea is it tries to find specific lines within the
config file and does a regex find and replace to swap in the theme name.

Happy to assist in your PR to answer questions.

### How to add custom themes to the set-theme script?

After installing these dotfiles you'll have a `~/.local/bin/set-theme` script.
It's a [zero dependency Python 3 script](./.local/bin/set-theme).

1. Open the above file
2. Check out the `THEMES` dictionary near the top of the file
3. Copy one of the existing themes' dictionary items, such as `tokyonight-moon` or `gruvbox-dark-medium`
    - If your theme has Neovim variants, copy Gruvbox else copy Tokyonight
4. Rename the dictionary's key to whatever your new theme's name is
    - If the Neovim theme name is the same as the dictionary key, that will be used
5. Create the associated `tmux` theme in `~/.config/tmux/themes`
6. Create the associated `fzf` theme in `~/.config/zsh/themes/fzf`
7. Create the associated `gitui` theme in `~/.config/gitui`
8. Modify any supported terminal configs to add the theme
9. Run `set-theme cooltheme`, replacing `cooltheme` with whatever name you used in step 4

If you added a theme with good contrast ratios please open a pull request to
get it added to the script.

### Where is the original Vim config?

I've made dozens of [blog posts and
videos](https://nickjanetakis.com/blog/tag/vim-tips-tricks-and-tutorials) about
Vim. Sometimes I linked directly to a commit so there's a permalink to it but
other times I did not.

Before switching to Neovim I made a `vim` git tag. You can check out the state
of the repo for that tag by [going
here](https://github.com/nickjj/dotfiles/tree/vim). You'll see `.vimrc` in the
root directory. If you cloned these dotfiles locally you can `git checkout
vim`. Keep in mind that's frozen to that point in time. Future updates
unrelated to Vim will not be included in that tag.

## 👀 About the Author

I'm a self taught developer and have been freelancing for the last ~20 years.
You can read about everything I've learned along the way on my site at
[https://nickjanetakis.com](https://nickjanetakis.com/). There's hundreds of
[blog posts](https://nickjanetakis.com/blog/) and a couple of [video
courses](https://nickjanetakis.com/courses/) on web development and deployment
topics. I also have a [podcast](https://runninginproduction.com) where I talk
to folks about running web apps in production.
