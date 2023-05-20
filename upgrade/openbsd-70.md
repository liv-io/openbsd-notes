# OpenBSD 7.0 Upgrade

## Download

```
rm -rf /usr/rel/
mkdir -p /usr/rel/
cd /usr/rel/

wget http://ftp.openbsd.org/pub/OpenBSD/7.0/amd64/SHA256
wget http://ftp.openbsd.org/pub/OpenBSD/7.0/amd64/SHA256.sig
wget http://ftp.openbsd.org/pub/OpenBSD/7.0/amd64/base70.tgz
wget http://ftp.openbsd.org/pub/OpenBSD/7.0/amd64/bsd
wget http://ftp.openbsd.org/pub/OpenBSD/7.0/amd64/bsd.mp
wget http://ftp.openbsd.org/pub/OpenBSD/7.0/amd64/bsd.rd
wget http://ftp.openbsd.org/pub/OpenBSD/7.0/amd64/man70.tgz

signify -C -p /etc/signify/openbsd-70-base.pub -x SHA256.sig base70.tgz
signify -C -p /etc/signify/openbsd-70-base.pub -x SHA256.sig bsd
signify -C -p /etc/signify/openbsd-70-base.pub -x SHA256.sig bsd.mp
signify -C -p /etc/signify/openbsd-70-base.pub -x SHA256.sig bsd.rd
signify -C -p /etc/signify/openbsd-70-base.pub -x SHA256.sig man70.tgz
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
tar -C / -xzphf man70.tgz
tar -C / -xzphf base70.tgz
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
rm -rf /usr/X11R6/lib/libdmx.* \
      /usr/X11R6/include/X11/extensions/dmxext.h \
      /usr/X11R6/lib/pkgconfig/dmx.pc \
      /usr/X11R6/man/man3/DMX*.3
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
