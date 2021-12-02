<img src="https://raw.githubusercontent.com/geerlingguy/mac-dev-playbook/master/files/Mac-Dev-Playbook-Logo.png" width="250" height="156" alt="Mac Dev Playbook Logo" />

# Mac Development Ansible Playbook for Frontend Developers

This playbook installs and configures most of the software used on Mac for software development. Some things in macOS are slightly difficult to automate, so there are still a few manual steps, but much less than if you were to install all the software yourself. Please review the main files in this repository and change them approiately to your needs:
  - main.yml - This is the main playbook file.
  - default.config.yml - Contains all software and VS Code extensions
  - dotfiles - You should review the repo that this links to as the dotfiles used may not be to your liking.
  - VS Code - You should review that the VSCode extensions in main.yml are what you would want and also update the user to your username (currently set as admin).

## Installation

  1. Ensure Apple's command line tools are installed (xcode-select --install to launch the installer).
  2. Run the run-setup.sh script to install the required software.
  3. Run `ansible-playbook main.yml --ask-become-pass` inside this directory. Enter your macOS account password when prompted for the 'BECOME' password.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `extra-packages` and `osx`.

    ansible-playbook main.yml -K --tags "dotfiles,homebrew"

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by just updating them in situ or by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

```yaml
homebrew_installed_packages:
  - cowsay
  - git
  - go

mas_installed_apps:
  - { id: 443987910, name: "1Password" }
  - { id: 498486288, name: "Quick Resizer" }
  - { id: 557168941, name: "Tweetbot" }
  - { id: 497799835, name: "Xcode" }

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

## Included Applications / Configuration (Default)

Applications (installed with Homebrew Cask):

  - [ChromeDriver](https://sites.google.com/chromium.org/driver/)
  - [Datagrip](https://www.jetbrains.com/datagrip/)
  - [Docker](https://www.docker.com/)
  - [Google Chrome](https://www.google.com/chrome/)
  - [Gradle](https://www.gradle.org/)
  - [Homebrew](http://brew.sh/)
  - [IntelliJ IDEA](https://www.jetbrains.com/idea/)
  - [iTerm2](https://www.iterm2.com/)
  - [MySql](https://www.mysql.com/)
  - [OpenJDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
  - [Postgresql](https://www.postgresql.org/)
  - [Postman](https://www.getpostman.com/)
  - [AWS Session Manager](https://aws.amazon.com/session-manager/)
  - [Slack](https://slack.com/)
  - [Sourcetree](https://www.sourcetreeapp.com/)
  - [Visual Studio Code](https://code.visualstudio.com/)
  - [Webstorm](https://www.jetbrains.com/webstorm/)

Packages (installed with Homebrew):
## *** Common Dev Tools ***
  - bash-completion
  - git
  - github/gh/gh
  - hub
  - mysql
  - nmap
  - node
  - nvm
  - openssl
  - readline
  - romkatv/powerlevel10k/powerlevel10k
  - ruby
  - sqlite
  - ssh-copy-id
  - tmux
  - wget
  - yarn
  - zsh-history-substring-search
  ## *** DevOps ***
  - awscli
  - jq
  - tree
  ## *** Java ***
  - gradle@6 # Build tool.
  - jenv # Manage your Java environment
  - jmeter # Load testing and performance measurement application
  - maven # Java-based project management

My [dotfiles](https://github.com/devunderslash/dotfiles) are also installed into the current user's home directory, including the `.osx` dotfile for configuring many aspects of macOS for better performance and ease of use. You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, there are a few other preferences and settings added on for various apps and services.
### Things that still need to be done manually

It's my hope that I can get the rest of these things wrapped up into Ansible playbooks soon, but for now, these steps need to be completed manually (assuming you already have Xcode and Ansible installed, and have run this playbook).

  1. Native MacOS terminal may need a font and theme update via preferences to take in changes made in .zshrc (MesloLGS NF Regular 13 and theme of your choice)
  2. Visual Studio Code may also need a font update via preferences:
    - Preferences > Editor > Font Size:  `"terminal.integrated.lineHeight": 1.3`
    - Preferences > Editor > Font Family: `"terminal.integrated.fontFamily": "MesloLGS NF"`
  3. For iTerm2, you may need to install a font via running `p10k configure` and then restart iTerm2.

## Testing the Playbook

This has been tested via Vagrant and VirtualBox using macOS Big Sur. Check the following repo with how to do this: #TODO

Use this repo to build a [Mac OS X VirtualBox VM](https://github.com/geerlingguy/mac-osx-virtualbox-vm), on which you can continually run and re-run this playbook to test changes and make sure things work correctly.

## Ansible for DevOps

Check out [Ansible for DevOps](https://www.ansiblefordevops.com/), which teaches you how to automate almost anything with Ansible.

## Author

This project was created by [Jeff Geerling](https://www.jeffgeerling.com/) (originally inspired by [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks)) and butchered by [Paul Devlin](https://github.com/devunderslash)

