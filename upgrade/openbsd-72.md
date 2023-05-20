# OpenBSD 7.2 Upgrade

## Download

```
rm -rf /usr/rel/
mkdir -p /usr/rel/
cd /usr/rel/

wget http://ftp.openbsd.org/pub/OpenBSD/7.2/amd64/SHA256
wget http://ftp.openbsd.org/pub/OpenBSD/7.2/amd64/SHA256.sig
wget http://ftp.openbsd.org/pub/OpenBSD/7.2/amd64/base72.tgz
wget http://ftp.openbsd.org/pub/OpenBSD/7.2/amd64/bsd
wget http://ftp.openbsd.org/pub/OpenBSD/7.2/amd64/bsd.mp
wget http://ftp.openbsd.org/pub/OpenBSD/7.2/amd64/bsd.rd
wget http://ftp.openbsd.org/pub/OpenBSD/7.2/amd64/man72.tgz

signify -C -p /etc/signify/openbsd-72-base.pub -x SHA256.sig base72.tgz
signify -C -p /etc/signify/openbsd-72-base.pub -x SHA256.sig bsd
signify -C -p /etc/signify/openbsd-72-base.pub -x SHA256.sig bsd.mp
signify -C -p /etc/signify/openbsd-72-base.pub -x SHA256.sig bsd.rd
signify -C -p /etc/signify/openbsd-72-base.pub -x SHA256.sig man72.tgz
```

## Kernel

### Single Processor

```
cd /usr/rel
ln -f /bsd /obsd && \cp bsd /nbsd && \mv /nbsd /bsd
\cp bsd.rd bsd.mp /
```

### Multiprocessor Processor

```
cd /usr/rel
ln -f /bsd /obsd && \cp bsd.mp /nbsd && \mv /nbsd /bsd
\cp bsd.rd /
\cp bsd /bsd.sp
```

## KARL

```
sha256 -h /var/db/kernel.SHA256 /bsd
```

## Userland

```
\cp /sbin/reboot /sbin/oreboot
tar -C / -xzphf man72.tgz
tar -C / -xzphf base72.tgz
```

## Reboot

```
/sbin/oreboot
```

## System and Device Files

```
cd /dev
./MAKEDEV all
```

## Bootloader

```
installboot sd0
```

## Update System

```
sysmerge
```

## Update firmware

```
fw_update
```

## Reboot

```
shutdown -r now
```

## Cleanup

```
userdel _switchd
groupdel _switchd
rm -f /etc/rc.d/switchd \
      /usr/sbin/switchctl \
      /usr/sbin/switchd \
      /usr/share/man/man4/switch.4 \
      /usr/share/man/man5/switchd.conf.5 \
      /usr/share/man/man8/switchctl.8 \
      /usr/share/man/man8/switchd.8
```

## Update packages

```
pkg_add -Uui
```

## Update configuration

```
syspatch
```

## Update Python

```
pkg_add python3
```

## Reboot

```
shutdown -r now
```
