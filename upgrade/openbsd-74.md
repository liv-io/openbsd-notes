# OpenBSD 7.4 Upgrade

# Upgrade

- Start a `tmux` session in case of connectivity issues

  ```
  tmux
  ```

- Download kernel and base archive

  ```
  rm -rf /usr/rel/
  mkdir -p /usr/rel/
  cd /usr/rel/

  wget http://ftp.openbsd.org/pub/OpenBSD/7.4/amd64/SHA256
  wget http://ftp.openbsd.org/pub/OpenBSD/7.4/amd64/SHA256.sig
  wget http://ftp.openbsd.org/pub/OpenBSD/7.4/amd64/base74.tgz
  wget http://ftp.openbsd.org/pub/OpenBSD/7.4/amd64/bsd
  wget http://ftp.openbsd.org/pub/OpenBSD/7.4/amd64/bsd.mp
  wget http://ftp.openbsd.org/pub/OpenBSD/7.4/amd64/bsd.rd
  wget http://ftp.openbsd.org/pub/OpenBSD/7.4/amd64/man74.tgz

  signify -C -p /etc/signify/openbsd-74-base.pub -x SHA256.sig base74.tgz
  signify -C -p /etc/signify/openbsd-74-base.pub -x SHA256.sig bsd
  signify -C -p /etc/signify/openbsd-74-base.pub -x SHA256.sig bsd.mp
  signify -C -p /etc/signify/openbsd-74-base.pub -x SHA256.sig bsd.rd
  signify -C -p /etc/signify/openbsd-74-base.pub -x SHA256.sig man74.tgz
  ```

- Replace kernel

> [!NOTE]
> Single Processor

  ```
  cd /usr/rel
  ln -f /bsd /obsd && \cp bsd /nbsd && \mv /nbsd /bsd
  \cp bsd.rd bsd.mp /
  ```

> [!NOTE]
> Multiprocessor Processor

  ```
  cd /usr/rel
  ln -f /bsd /obsd && \cp bsd.mp /nbsd && \mv /nbsd /bsd
  \cp bsd.rd /
  \cp bsd /bsd.sp
  ```

- Enable KARL

  ```
  sha256 -h /var/db/kernel.SHA256 /bsd
  ```

- Update userland

  ```
  \cp /sbin/reboot /sbin/oreboot
  tar -C / -xzphf man74.tgz
  tar -C / -xzphf base74.tgz
  ```

- Reboot

  ```
  /sbin/oreboot
  ```

- Start a `tmux` session in case of connectivity issues

  ```
  tmux
  ```

- Update dev

  ```
  cd /dev
  ./MAKEDEV all
  ```

- Update bootloader

  ```
  installboot sd0
  ```

- Update system configuration files

  ```
  sysmerge
  ```

- Update firmware

  ```
  fw_update
  ```

- Update packages

  ```
  pkg_add -Uui
  ```

- Apply system binary patches

  ```
  syspatch
  ```

- Run Ansible playbook to ensure correct configuration

  ```
  ansible-playbook all.yml --limit=<fqdn>
  ansible-playbook firewall.yml --limit=<fqdn>
  ```

- Reboot system

  ```
  sleep 3 && shutdown -r now
  ```

- Inspect logs

  ```
  grep -IirE "fatal|emerg|alert|crit|err|warn|corrupt|fail" /var/log/
  ```
