# sheepnet-dkms

This is basically the [macemu BasiliskII Linux NetDriver](https://github.com/cebix/macemu/tree/master/BasiliskII/src/Unix/Linux/NetDriver) needed for BasiliskII (and probably several other Macintosh emulators) to do bridged networking with a physical adapter.

You can maybe debuild this repo or you can just be lazy:
```
sudo curl https://apt.hatestheinternet.com/apt.hatestheinternet.com.gpg -o /etc/apt/trusted.gpg.d/apt.hatestheinternet.com.gpg
echo 'deb [arch=amd64] http://apt.hatestheinternet.com retro main' | sudo tee /etc/apt/sources.list.d/hti-retro.list
sudo apt update && sudo apt install sheepnet-dkms
```

This module does not auto-install, so before running Basilisk (or whatever), you need to:
```
sudo modprobe sheep_net
```

If some kind soul wants to submit a pull request with a udev rule I will accept it but I'm lazy so, until then:
```
sudo chown root:adm /dev/sheep_net
sudo chmod 660 /dev/sheep_net
```

After this, once you load Basilisk (so long as your user is in that `adm` group), just choose the network interface you want to bridge:

![Basilisk "Serial/Network settings" bound to local ethernet interface "eno1"](https://github.com/hatestheinternet/sheepnet-dkms/blob/trunk/images/basilisk-net.png?raw=true)

So long as everything else is set up properly, here's a System 7.5.3 Macintosh VM mounting the SYS share off a Netware server over AppleTalk/DDP:

![A System 7.5.3 virtual Macintosh mounting the SYS volume off a Novell Netware server over AppleTalk/DDP](https://github.com/hatestheinternet/sheepnet-dkms/blob/trunk/images/netware.png?raw=true)

## Secure Boot

If your (virtual) machine is using secure boot and this is your first DKMS module you may have to approve a key. Basically, if this happens:

![The first DKMS module built on a secure boot system asking for a password](https://github.com/hatestheinternet/sheepnet-dkms/blob/trunk/images/secboot.png?raw=true)

Just remember the crap password you set because, when you reboot that machine, you need to wander down the "Enroll MOK" path and it will eventually ask for it:

![The first step adding a new MOK when an EFI Proxmox virtual machine with secure boot enabled ... umm, reboots](https://github.com/hatestheinternet/sheepnet-dkms/blob/trunk/images/efimok.png?raw=true)

**NOTE** If this is indeed your first time, you will not be able to load the module until you've rebooted and enrolled the key.

**DOUBLE NOTE** Don't disable secure boot, do it properly.

