# Development setup

## Disclaimer

This is an opiniated setup, no need to follow it to the letter.

## Parts & steps

- [Ubuntu](#ubuntu)
- ["Oh My ZSH!"](#oh-my-zsh)
- [Docker](#docker)
- [GUI Tools](#gui-tools)

## Ubuntu

As of writing the latest stable version is Ubuntu 19.10.

After an (_installation_|_release-upgrade_) of the bare minimum system, update and upgrade it and then install some needed packages:

```sh
sudo apt update
sudo apt upgrade
sudo apt dist-upgrade
sudo apt install curl git zsh 
```
Add packages you'd like eg. `gnome-software-plugin-flatpak`, etc.

## "Oh my zsh!"

"Oh my zsh!" is a nice visual and usefult setup for the zsh. Even though many tools in this setup is GUI software, the times I'm in the shell I want eye candy, history completion and command completion.

First, follow the [installation instructions for `oh-my-zsh`](https://ohmyz.sh/).

Secondly, add at the end of your .zshrc file the path to the `./.local/bin` directory (needed for further installations):

```
export PATH=$HOME/.local/bin:$PATH
```

At the same occasion one should activate the following plugins:

```sh
plugins=(git docker docker-compose command-not-found composer history history-substring-search)
```

Add your desired plugins here, too (eg. `npm`, `flutter`, etc).

## Docker

As of writing there's no official package from Docker available (for Ubuntu 19.10) and the docker.io package is advised.

```sh
sudo apt install docker.io docker-compose
```

### Setup without the need to use sudo

Check existance of the _docker_ group
```sh
sudo groupadd docker
```

Add the current user to the _docker_ group
```sh
sudo gpasswd -a $USER docker
```

_Log out and log in to activate the changes!_

Test the permissions

```sh
docker run hello-world
```

## GUI Tools

Even though one can do everything in the terminal, some visual tools are nice also:

- [DockStation](#dockstation)
- [GitKraken](#gitkraken)
- [Postman](#postman)
- [JetBrains Toolbox](#jetbrains-toolbox)

### DockStation

A nifty tool to check visually the actual state of the docker containers (and as an alternative to the discontinued Kitematic) [get DockStation here](https://dockstation.io/).

### GitKraken

A non-free tool to visualize everything git related.
Try it either as snap package `snap install gitkraken` or [download it from the website](https://www.gitkraken.com/).

### Postman

Postman, the tool one needs to develop and test APIs, can also be installed as snap package `snap install postman` (I'd advise it) or be [downloaded on the website](https://www.getpostman.com/).

### JetBrains Toolbox

Even though PhpStorm is available as a snap package, I prefer the Toolbox.
[Download the toolbox binary](https://www.jetbrains.com/toolbox-app/download/) and put it into the _bin_ directory: `mv jetbrains-toolbox $HOME/.local/bin/`. Run it and install PhpStorm (and any other software you have subscribed to).

