# OpenBSD 7.1 Upgrade

## Download

```
rm -rf /usr/rel/
mkdir -p /usr/rel/
cd /usr/rel/

wget http://ftp.openbsd.org/pub/OpenBSD/7.1/amd64/SHA256
wget http://ftp.openbsd.org/pub/OpenBSD/7.1/amd64/SHA256.sig
wget http://ftp.openbsd.org/pub/OpenBSD/7.1/amd64/base71.tgz
wget http://ftp.openbsd.org/pub/OpenBSD/7.1/amd64/bsd
wget http://ftp.openbsd.org/pub/OpenBSD/7.1/amd64/bsd.mp
wget http://ftp.openbsd.org/pub/OpenBSD/7.1/amd64/bsd.rd
wget http://ftp.openbsd.org/pub/OpenBSD/7.1/amd64/man71.tgz

signify -C -p /etc/signify/openbsd-71-base.pub -x SHA256.sig base71.tgz
signify -C -p /etc/signify/openbsd-71-base.pub -x SHA256.sig bsd
signify -C -p /etc/signify/openbsd-71-base.pub -x SHA256.sig bsd.mp
signify -C -p /etc/signify/openbsd-71-base.pub -x SHA256.sig bsd.rd
signify -C -p /etc/signify/openbsd-71-base.pub -x SHA256.sig man71.tgz
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
tar -C / -xzphf man71.tgz
tar -C / -xzphf base71.tgz
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

## Update Firmware

```
fw_update
```

## Reboot

```
shutdown -r now
```

## Cleanup

## Update Packages

```
pkg_add -Uui
```

## Update Configuration

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
