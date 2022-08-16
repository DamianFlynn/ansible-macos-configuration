<img src="https://raw.githubusercontent.com/geerlingguy/mac-dev-playbook/master/files/Mac-Dev-Playbook-Logo.png" width="250" height="156" alt="Mac Dev Playbook Logo" />

# Mac Development Ansible Playbook

[![CI][badge-gh-actions]][link-gh-actions]

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have a few manual installation steps, but at least it's all documented here.

## Installation

This playbook can be used on an fresh install of MacOS (Greenfield) or an currently in use environment (Brownfield)

  1. Launch Terminal

  1. Install Homebrew
      1. Run the following command```/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"```
      1. Homebrew will install as part of its toolkit, Apple's command line development tools called xcode

  1. Install Ansible
     1. Run the following command `brew install ansible`

  1. Install Oh-My-Zsh
      1. Run the following command `sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
      1. Download the *Powerlevel10k* theme with the command `git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k`
      1. Download and install the *zsg-autosuggestions* plugin with the command `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
      1. Download and install the *zsg-syntax-highlighting* plugin with the command `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`
      1. Download and install the *1Password* plugin with the command `git clone https://github.com/sirhc/op.plugin.zsh.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/op`

  1. Clone or download this repository to your local drive.
      1. Create a Developer folder, with `mkdir ~/Developer && cd ~/Developer`
      1. Clone the Playbook repository with `git clone https://github.com/DamianFlynn/ansible-macos-configuration.git`
      1. Change in the playbook folder using `cd ansible-macos-configuration`
      1. Run `ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
      1. Run `ansible-playbook main.yml --ask-become-pass` inside this directory. Enter your macOS account password when prompted for the 'BECOME' password.

  > Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

## What's Gets Installed

### Fonts

#### Monoid

- Monoisome-Regular.ttf

#### Meslo LG M for Powerline

- Meslo LG L Bold Italic for Powerline.ttf
- Meslo LG L Bold for Powerline.ttf
- Meslo LG L Italic for Powerline.ttf
- Meslo LG L Regular for Powerline.ttf
- Meslo LG M Bold Italic for Powerline.ttf
- Meslo LG M Bold for Powerline.ttf
- Meslo LG M Italic for Powerline.ttf
- Meslo LG M Regular for Powerline.ttf
- Meslo LG S Bold Italic for Powerline.ttf
- Meslo LG S Bold for Powerline.ttf
- Meslo LG S Italic for Powerline.ttf
- Meslo LG S Regular for Powerline.ttf

### dotfiles

Cloned from my [dotfile repo](https://github.com/DamianFlynn/dotfiles.git)

- .zshrc
- .gitignore
- .gitconfig
- .aliases
- .inputrc
- .osx
- .vimrc

### Applications

#### Command Line (Homebrew)

- autoconf
- bash-completion
- gettext
- gifsicle
- git
- github/gh/gh
- go
- gpg
- httpie
- iperf
- mcrypt
- nmap
- node
- ssh-copy-id
- readline
- openssl
- pv
- wget
- wrk
- zsh-history-substring-search
- azure-cli
- brew-cask-completion
- curl
- exiftool
- flux
- packer
- hugo
- jq
- helm
- kompose
- telnet
- k9s
- terraform
- kubernetes-cli
- tmux
- mas
- bicep

#### Graphical Apps (Homebrew)

- docker
- iterm2
- google-chrome
- vagrant
- microsoft-teams
- powershell
- microsoft-edge
- visual-studio-code
- obsidian
- elgato-stream-deck
- elgato-wave-link
- hdhomerun
- yubico-yubikey-manager
- 1password-beta
- 1password-cli-beta

#### Graphical Apps (Mac App Store)

- Cardhop
- MQTT Explorer
- Microsoft Word
- Microsoft PowerPoint
- Microsoft Excel
- Microsoft Outlook
- OneDrive
- XMind: Mind Mapping
- Pluralsight: Learn Tech Skills
- WhatsApp Desktop
- Home Assistant
- The Unarchiver



# Manual Steps

## Operating System settings

Settings -> Preferences -> Mission Control

- [x] Automatically rearrange spaces based on most recent use

## iTerm

In the Preferences window, select profiles

### Appearance

#### General

Status bar location, set to *Bottom*

### Keys

#### Hotkeys

Enable *Hotkeys*, and set the following options

- Hotkey to use, in my case this is CTRL + `
- [ ] Pin hotkey window
- [ ] Automatically reopen on app reactivation
- [x] Animate showing and hiding
- [x] Floating Window

### Profiles

The following can be applied on the **Default** Profile

#### Colors

In the *Color presets...* dropdown, select **Import** and load the file `~/Developer/dotfiles/theme.itermcolors`. Then in the same *Color Presets...* dropdown select the new **theme**

#### Text

Set the Font to *Meslo LG L for Powerline*, weight as *Regular*, with *Ligatures* enabled

#### Session

Set the *Status bar* as enabled and then configure the status bar as you wish, typically, I add

#### Window

Settings for new Windows
On the **Hotkey Window** Profile

- Columns - 132
- Rows - 25
- Set Style to *Normal*
- Set Screen to *Screen with Cursor*
- Set Space to *All Spaces*

## 1Password

In the Preferences window of the application we should apply the following

- CPU Utilization
- Memory Utilization
- Network Troughtput
- Git state

### General

- [x] Start at login
- [x] Format secure notes using Markdown

### Developer

- [x] Use the SSH Agent
- [x] Display key names when authorizing connections
- [x] Biometric unlock for 1Password CLI

## SSH

### Enable 1Password as the SSH Agent

1Password now includes to option of hosting the private keys for our SSH collection. With the private keys stored in the vault, we can proceed to configure the ssh agent on MacOS to use 1Password as the provider

Edit the configuration file with `vi ~/.ssh/config` and add the following content:

```conf
Host *
  IdentityAgent "~/Library/Group Containers/2BUA8C4S2C.com.1password/t/agent.sock"
```

## Wave Link

Elgato Wave 3

VST

### Renegate

Gain Map -30db with 100% Gain Cut
Detector at -29db
Envelope 30ms Attach, Hold for 70ms, and Release at 20ms

### T-De-Esser

Processing at -32db
Intensity at 7.0
Sharpnet at -7.0
Frequency High

### Nova Equalizer

| Q | Freq | Gain |
|---|---|---|
|1.0| 115 Hz| 3.0db|
|1.0| 230 Hz| -0.5db |
|0.5| 1000 Hz | -1.5db |
|0.60|3.2 kHz| 4.5db |

### Frontier Limitier

Release - Fast
Output Level -3.5db
Threshold -8.0db


## VS Code Extensions

Extensions currently installed in VS Code

- ms-toolsai.jupyter-renderers
- ms-dotnettools.vscode-dotnet-runtime
- ms-toolsai.jupyter-keymap
- ms-azuretools.vscode-azureresourcegroups
- davidanson.vscode-markdownlint
- signageos.signageos-vscode-sops
- ms-vscode-remote.remote-ssh-edit
- ms-python.vscode-pylance
- ms-azuretools.vscode-azurelogicapps
- ms-vscode-remote.remote-ssh
- ms-vscode.azurecli
- ms-azuretools.vscode-azurefunctions
- ms-vscode.powershell
- tintinweb.graphviz-interactive-preview
- eamodio.gitlens
- ms-kubernetes-tools.vscode-kubernetes-tools
- redhat.ansible
- redhat.vscode-yaml
- ms-vscode.azure-account
- ms-python.python
- ms-kubernetes-tools.vscode-aks-tools
- hashicorp.terraform
- ms-toolsai.jupyter
- ms-azuretools.vscode-bicep
- github.vscode-pull-request-github
- github.copilot
- github.remotehub
- ms-vscode.remote-repositories

## Virtual Machines

### Vagrant

Make sure QEMU is installed, if not:

`brew install qemu vagrant`

Install plugin:

`vagrant plugin install vagrant-qemu`

Prepare a Vagrantfile, see Example, and start:

`vagrant up --provider qemu`

### Multipass

Install Multipass

```bash
sudo multipass set local.driver=qemu
uname -vp
Darwin Kernel Version 20.6.0: Tue Feb 22 21:10:42 PST 2022; root:xnu-7195.141.26~1/RELEASE_ARM64_T8101 arm

$ multipass networks
Name     Type         Description
bridge0  bridge       Network bridge with en1, en2
en0      wifi         Wi-Fi
en1      thunderbolt  Thunderbolt 1
en2      thunderbolt  Thunderbolt 2
en3      ethernet     Ethernet Adaptor (en3)
en4      ethernet     Ethernet Adaptor (en4)

$ multipass find
Image                       Aliases           Version          Description
snapcraft:core18            18.04             20201111         Snapcraft builder for Core 18
snapcraft:core20            20.04             20210921         Snapcraft builder for Core 20
core                        core16            20200818         Ubuntu Core 16
core18                                        20211124         Ubuntu Core 18
18.04                       bionic            20220513         Ubuntu 18.04 LTS
20.04                       focal,lts         20220505         Ubuntu 20.04 LTS
21.10                       impish            20220309         Ubuntu 21.10
appliance:adguard-home                        20200812         Ubuntu AdGuard Home Appliance
appliance:mosquitto                           20200812         Ubuntu Mosquitto Appliance
appliance:nextcloud                           20200812         Ubuntu Nextcloud Appliance
appliance:openhab                             20200812         Ubuntu openHAB Home Appliance
appliance:plexmediaserver                     20200812         Ubuntu Plex Media Server Appliance
anbox-cloud-appliance                         latest           Anbox Cloud Appliance
charm-dev                                     latest           A development and testing environment for charmers
docker                                        latest           A Docker environment with Portainer and related tools
minikube                                      latest           minikube is local Kubernetes

  <command> <action> <Image> <bridge network> <name>   <cpus> <ram> <disk>
$ multipass launch   21.10   --network en0    -n node1 -c 2   -m 4G -d 50G
Launched: node1

multipass launch 22.04 -n primary -c 2 -m 4G -d 50G

$ ping node1
PING node1.local (10.2.0.39): 56 data bytes
64 bytes from 10.2.0.39: icmp_seq=0 ttl=63 time=7.727 ms
64 bytes from 10.2.0.39: icmp_seq=1 ttl=63 time=15.083 ms
64 bytes from 10.2.0.39: icmp_seq=2 ttl=63 time=15.719 ms
^C
--- node1.local ping statistics ---
3 packets transmitted, 3 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 7.727/12.843/15.719/3.627 ms

$ multipass list

$ multipass shell

>$ sudo passwd ubuntu
>$ sudo adduser username
>$ sudo usermod -aG sudo username
>$ ip a
```


   ╭─────────────────────────────────────────────────────────────────╮
   │░░░░░░░░░░░░░░░░░░░░░░░░░░░ Next Steps ░░░░░░░░░░░░░░░░░░░░░░░░░░│
   ├─────────────────────────────────────────────────────────────────┤
   │                                                                 │
   │   There are still a few steps you need to do to finish setup.   │
   │                                                                 │
   │        The link below has Post Installation Instructions        │
   │                                                                 │
   └─────────────────────────────────────────────────────────────────┘


### Use with a remote Mac

You can use this playbook to manage other Macs as well; the playbook doesn't even need to be run from a Mac at all! If you want to manage a remote Mac, either another Mac on your network, or a hosted Mac like the ones from [MacStadium](https://www.macstadium.com), you just need to make sure you can connect to it with SSH:

  1. (On the Mac you want to connect to:) Go to System Preferences > Sharing.
  2. Enable 'Remote Login'.

> You can also enable remote login on the command line:
>
>     sudo systemsetup -setremotelogin on

Then edit the `inventory` file in this repository and change the line that starts with `127.0.0.1` to:

```
[ip address or hostname of mac]  ansible_user=[mac ssh username]
```

If you need to supply an SSH password (if you don't use SSH keys), make sure to pass the `--ask-pass` parameter to the `ansible-playbook` command.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `extra-packages` and `osx`.

    ansible-playbook main.yml -K --tags "dotfiles,homebrew"

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

```yaml

composer_packages:
  - name: hirak/prestissimo
  - name: drush/drush
    version: '^8.1'

gem_packages:
  - name: bundler
    state: latest

npm_packages:
  - name: webpack

pip_packages:
  - name: mkdocs

configure_dock: true
dockitems_remove:
  - Launchpad
  - TV
dockitems_persist:
  - name: "Sublime Text"
    path: "/Applications/Sublime Text.app/"
    pos: 5
```

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.


My [dotfiles](https://github.com/damianflynn/dotfiles) are also installed into the current user's home directory, including the `.osx` dotfile for configuring many aspects of macOS for better performance and ease of use. You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, there are a few other preferences and settings added on for various apps and services.

## Future additions

### Things that still need to be done manually

It's my hope that I can get the rest of these things wrapped up into Ansible playbooks soon, but for now, these steps need to be completed manually (assuming you already have Xcode and Ansible installed, and have run this playbook).



## Author

This project was created by [Jeff Geerling](https://www.jeffgeerling.com/) (originally inspired by [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks)).

[badge-gh-actions]: https://github.com/geerlingguy/mac-dev-playbook/workflows/CI/badge.svg?event=push
[link-gh-actions]: https://github.com/geerlingguy/mac-dev-playbook/actions?query=workflow%3ACI
