# Installing CoreOS to Disk

## Boot base system

Boot Arch Linux live CD or similar

## Install Script

This repository contains a simple installer that will destroy everything on the given target disk and install CoreOS.
Essentially it downloads an image, verifies it with gpg and then copies it bit for bit to disk.

The script is self-contained and located [on Github here](https://raw.github.com/domabo/coreos-install/master/coreos-install "coreos-install").

```
wget https://raw.githubusercontent.com/domabo/coreos-install/master/coreos-install
chmod +x coreos-install
wget https://raw.githubusercontent.com/domabo/coreos-install/master/config
nano config
coreos-install -d /dev/vda -V alpha -c ./config
```

When running on CoreOS the install script will attempt to install the same version. If you want to ensure you are installing the latest available version use the `-V` option:

```
coreos-install -d /dev/vda -V alpha
```

For reference here are the rest of the `coreos-install` options:

    -d DEVICE   Install CoreOS to the given device.
    -V VERSION  Version to install (e.g. alpha)
    -o OEM      OEM type to install (e.g. openstack)
    -c CLOUD    Insert a cloud-init config to be executed on boot.
    -t TMPDIR   Temporary location with enough space to download images.

## Cloud Config

Pass the config file to `coreos-install` via the `-c` option.  It automatically downloads SSH keys from the Github user specified in the file.

It will be installed to `/var/lib/coreos-install/user_data` and evaluated on every boot.

```
coreos-install -d /dev/vda -c ./config
```


## Manual Tweaks

If cloud config doesn't handle something you need to do or the config file does not get created and you just want to take a look at the root btrfs filesystem before booting your new install just mount the ninth partition:

```
mount -o subvol=root /dev/vda9 /mnt/
```


## Command line

INSTALLING THE CLI TO CONTROL COREOS

If you are on a Mac, you can install fleetctl and etcdctl natively to control your CoreOS clusters. Here is how:

```
$ brew install go etcdctl
$ git clone https://github.com/coreos/fleet.git
$ cd fleet
$ ./build
$ mv bin/fleetctl /usr/local/bin/
ssh-add ~/.ssh/id_rsa
```


