<img src="https://raw.githubusercontent.com/geerlingguy/mac-dev-playbook/master/files/Mac-Dev-Playbook-Logo.png" width="250" height="156" alt="Mac Dev Playbook Logo" />

# Mac Development Ansible Playbook

[![CI][badge-gh-actions]][link-gh-actions]

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have a few manual installation steps, but at least it's all documented here.

## Installation

This playbook can be used on an fresh install of MacOS (Greenfield) or an currently in use environment (Brownfield)

  1. Launch Terminal

  2. Install Homebrew
     1. Run the following command```/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"```
     1. Homebrew will install as part of its toolkit, Apple's command line development tools called xcode

  2. Install Ansible
     1. Run the following command `brew install ansible`
     
  3. Install Oh-My-Zsh
     1. Run the following command `sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

  3. Clone or download this repository to your local drive.
     1. Create a Developer folder, with `mkdir ~/Developer && cd ~/Developer`
     2. Clone the Playbook repository with `git clone https://github.com/DamianFlynn/ansible-macos-configuration.git`
     3. Change in the playbook folder using `cd ansible-macos-configuration`
     4. Run `ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
     5. Run `ansible-playbook main.yml --ask-become-pass` inside this directory. Enter your macOS account password when prompted for the 'BECOME' password.

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
Cloned from my repo https://github.com/DamianFlynn/dotfiles.git
- .zshrc
- .gitignore
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

   ╭─────────────────────────────────────────────────────────────────╮
   │░░░░░░░░░░░░░░░░░░░░░░░░░░░ Next Steps ░░░░░░░░░░░░░░░░░░░░░░░░░░│
   ├─────────────────────────────────────────────────────────────────┤
   │                                                                 │
   │   There are still a few steps you need to do to finish setup.   │
   │                                                                 │
   │        The link below has Post Installation Instructions        │
   │                                                                 │
   └─────────────────────────────────────────────────────────────────┘
# Manual Steps

## Operating System settings
Settings -> Preferences -> Mission Control

- [x] Automatically rearrange spaces based on most recent use

## 1Password
In the Preferences window of the application we should apply the following

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

```
Host *
	IdentityAgent "~/Library/Group Containers/2BUA8C4S2C.com.1password/t/agent.sock"
```


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
