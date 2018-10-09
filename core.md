---
title: core
image: axecore-ascii-screenshot.png
---
<section><ul class="actions">
<li><a href="https://github.com/AXErunners/axe/releases/latest" class="button icon fa-download">binaries</a></li>
</ul>
</section>
<h2>Deployment</h2>
Following guide covers AXE core compilation and was tested on Ubuntu 18.04.
<blockquote>To update the current client with a fresh version you need to shut down current AXE client with <code>axe-cli stop</code> before using any gists.</blockquote>
<h3>Prepare the system</h3>
Update operating system and install dependencies:
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git python-virtualenv virtualenv fail2ban ufw -y
sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils -y
sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler -y
sudo apt-get install libboost-all-dev libminiupnpc-dev libzmq3-dev libqrencode-dev -y
sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install libdb4.8-dev libdb4.8++-dev -y
```
<h3>Install AXE core and launch the client</h3>

Use the one-liner to install AXE core:

```
wget https://gist.github.com/charlesrocket/f5331e54b47344b6957781bbbea8dc33/raw/17e4d3d1ce8ee5e45b5b022c32d7fa2616ba5643/axecore.sh && bash axecore.sh
```

Now start GUI client with `axe-qt` or headless version with `axed`.<br />
<br />

<hr class="hr-line">
<h2>Gitian builds</h2>
<img src="https://raw.githubusercontent.com/AXErunners/media/master/etc/axe-gitian-mojave.png" width="425">

Use [AXE-gitian](https://github.com/AXErunners/axe-gitian) to perform deterministic binary builds using [Vagrant](https://www.vagrantup.com/) and [VirtualBox](https://www.virtualbox.org/) with [Ansible](https://www.ansible.com/) on macOS. Gitian provides a way to be reasonably certain that the AXE executables are really built from the exact source on GitHub and have not been tampered with. It also makes sure that the same, tested dependencies are used and statically built into the executable.

Multiple developers build from source code by following a specific descriptor ("recipe"), cryptographically sign the result, and upload the resulting signature. These results are compared and only if they match is the build is accepted.

<h4>Vagrant</h4>

Download and install the latest version of Vagrant from their [website](https://www.vagrantup.com/downloads.html).

<h4>VirtualBoX</h4>

Download and install the latest version of VirtualBox from their [website](https://www.virtualbox.org/wiki/Downloads).

<h4>Ansible</h4>

Install prerequisites
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew doctor
brew install git
sudo easy_install pip
```
Setup Ansible

`sudo pip install ansible`

<h3>axe-gitian</h3>
```
git clone https://github.com/AXErunners/axe-gitian
cd axe-gitian
```
Edit your `gitian.yml` file:
```
# URL of repository containing AXE source code.
axe_git_repo_url: 'https://github.com/AXErunners/axe'

# Specific tag or branch you want to build.
axe_version: 'master'

# The name@ in the e-mail address of your GPG key, alternatively a key ID.
gpg_key_name: 'F16219F4C23F91112E9C734A8DFCBF8E5A4D8019'

# OPTIONAL set to import your SSH key into the VM. Example: id_rsa, id_ed25519. Assumed to reside in ~/.ssh
ssh_key_name: ''
```
Start the build with `vagrant up --provision axe-build`. When environment is ready - connect with `vagrant ssh axe-build` and download Apple [SDK](https://github.com/AXErunners/axe/blob/master/doc/README_osx.md) into `gitian-builder/inputs` (`wget` + dropbox).

Then prepare container and start the build with:
```
#replace $SIGNER and $VERSION to match your gitian.yml
./gitian-build.py --setup $signer $version
./gitian-build.py -B $SIGNER $VERSION
```

Copy files with [plugin](https://github.com/AXErunners/axe-gitian#copying-files).

<i>guide for [linux](https://github.com/AXErunners/axe-gitian#requirements)</i>

<hr class="hr-line">
<h2>Gist examinations</h2>
<h3>Local</h3>

AXE core one-liner gist downloads master branch, performs dependencies build, system configuration, compilation with standard parameters and installation info `bin` folder. Perfect for fresh systems.

* `git clone` - downloads the source code
* `./autogen` - preparing system for the source code compilation
* `./configure` - preparing environment
* `make` - build process
* `sudo make install` - copies binaries into `bin` folder for quick launch (`axed`/`axe-qt`/etc can be called from any directory) _optional_

{% gist f5331e54b47344b6957781bbbea8dc33 %}

<h3>Server</h3>

AXE core one-liner for VPS will perform same tasks but with the **headless** flag (there is no need for GUI on the server machine). You can ignore **Qt** dependencies and call the gist with:
```
wget https://gist.github.com/charlesrocket/675ae3d744aed0d06852fc1dbf6f4739/raw/b9f09174e055a96880e27dfeba8bdff994c03225/axecore-vps.sh && bash axecore-vps.sh
```
{% gist 675ae3d744aed0d06852fc1dbf6f4739 %}

<h2>Outro</h2>
For additional services check with [Masternodes](/#masternodes)  or [p2pool](/#p2pool) section.