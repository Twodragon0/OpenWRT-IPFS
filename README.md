
# IPFS installation in Raspbian OS

Log into your system with an administrator account, like the default OS user. For example, on the Raspberry Pi, most
operating systems will default to the `pi` user, whereas on the Orange Pi that's going to be `orangepi`. From any local directory, clone or download this repo, `cd` into it and run the installer:

```SHELL
./install
```

### Notes

* Do **not** execute the installation script with `sudo`
* You'll need root privileges to run the installer. The default OS user (`pi`, `orangepi` etc.) does so by default
* The IPFS user directory will be created at `~/.ipfs` (eg.: `/home/pi/.ipfs`, `/home/orangepi/.ipfs` etc.)

### Installation options

You can specify a version for IPFS (eg.: `v0.4.23`):

```SHELL
./install v0.4.23
```

# IPFS installaiton on OpenWRT Router

A bare bones [IPFS](https://ipfs.io) installer using OpenWRT for the Raspberry Pi and other ARM-based devices.

## Requirement

```SHELL
opkg update && opkg install git git-http curl wget bash
```

### Error Solution

* ./lib/functions.sh: line 12: rpm: command not found
* ./install: line 58: whoami: command not found
* ./install: line 61: [: ==: unary operator expected
* ./install: line 74: [: ==: unary operator expected
* >>> Unable to detect init system - you don't seem to be using systemd or upstart. The IPFS daemon will have to be controlled manually.

```sh
mv ~/OpenWRT-IPFS/ipfs /usr/bin/
``` 

# Go-ipfs installation on Untangle Router

```sh
sudo apt-get update
sudo apt-get install golang-go -y
wget https://dist.ipfs.io/go-ipfs/v0.4.23/go-ipfs_v0.4.23_linux-amd64.tar.gz
tar xvfz go-ipfs_v0.4.23_linux-amd64.tar.gz
sudo mv go-ipfs/ipfs /usr/local/bin/ipfs
```

# Go-ipfs installation on pfSense Router

Installing go-ipfs on FreeBSD-based pfSense is similar to running go-ipfs on any UNIX based system. First, go to Golang.org/dl to download the latest stable version of go and go-ipfs binary for FreeBSD. Then run the following commands on your system.

```sh
pkg update
pkg install curl git
cd /tmp
curl -O https://dl.google.com/go/go1.14.freebsd-amd64.tar.gz
curl -O https://dist.ipfs.io/go-ipfs/v0.4.23/go-ipfs_v0.4.23_freebsd-amd64.tar.gz
tar -C /usr/local -xzf go1.14.freebsd-amd64.tar.gz
tar -C go-ipfs_v0.4.23_freebsd-amd64.tar.gz
```

Then create the $path to the environment variable to create the following folders.

```sh
mkdir ~/.gopkg
setenv GOPATH /root/.gopkg
set path = ($path /usr/local/go/bin /root/.gopkg/bin)
```
Please reboot and then you can see go and go-ipfs command.


## IPFS usage

You can find a lot of information on how to use IPFS on the [official website](https://ipfs.io/docs/getting-started/).
If you just want to test whether the installation was successful or not, you can list your node's peers:

```SHELL
ipfs swarm peers
```

## IPFS daemon

The IPFS daemon needs to be running in order for your IPFS node to appear online. The installer already takes care of
running the daemon on system startup by default, but if you want to control that process manually, you can use the
operating system's init system directly.

For `systemd` (Raspbian Stretch, Ubuntu 18.04. and newer, CentOS 7 and newer), you can use:

```SHELL
systemctl {start|status|stop} ipfs-daemon.service
```

For `upstart` (Ubuntu 9.10 to Ubuntu 14.10, Centos 6), you can use:

```SHELL
service ipfs-daemon {start|status|stop}
```

## Uninstallation

In order to uninstall IPFS, just execute the uninstaller and follow the uninstallation steps:

```SHELL
./uninstall
```

## Upgrade

If you want to upgrade to a newer version, run the installer again.

## Support matrix

| SBC/ARM device    | Raspbian Stretch  | Ubuntu 14.04  |
| :---------------- | :---------------- | :------------ |
| Raspberry Pi 0    | Not tested        | Not tested    |
| Raspberry Pi 1    | Yes               | Not tested    |
| Raspberry Pi 2    | Yes               | Not tested    |
| Raspberry Pi 3    | Yes               | Not tested    |
| Orange Pi         | Not tested        | Yes           |

## How to contribute

* for bug reports, open a new issue
* for code patches, open a pull request against the `development` branch
* for bugs specific to IPFS, please refer to the [official channel](https://discuss.ipfs.io)
