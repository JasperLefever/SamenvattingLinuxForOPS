# Important files and directories in

## In general

- `/etc/...`: configuration files
- `/var/...`: variable data -> log files, mail, etc.
- `/usr/...`: user related programs
- `/bin/...`: binaries (programs)

## Important files (general)

| File                         | Description                                                                                                |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `/etc/passwd`                | user account information -> use `vipw` for proper locking                                                  |
| `/etc/group`                 | group information -> use `vigr` for proper locking                                                         |
| `/etc/shadow`                | encrypted passwords -> use `vipw` for proper locking                                                       |
| `/etc/default/useradd`       | default values for `useradd` command                                                                       |
| `/etc/skel`                  | skeleton directory for new users , all files in this directory are copied to the new user's home directory |
| `/etc/login.defs`            | login definitions, password length, account expiration, etc.                                               |
| `/etc/sudoers`               | sudo configuration (who has sudo rights)                                                                   |
| `/etc/hosts`                 | IP addresses and hostnames                                                                                 |
| `/etc/motd`                  | message of the day                                                                                         |
| `/etc/issue`                 | prelogin message                                                                                           |
| `/etc/shells`                | list of valid shells                                                                                       |
| `/etc/protocols`             | list of valid IP protocols                                                                                 |
| `/etc/services`              | list of valid services (portnumbers)                                                                       |
| `/etc/resolv.conf`           | DNS configuration (do not edit)                                                                            |
| `/etc/systemd/resolved.conf` | DNS configuration for systemd-resolved                                                                     |
| `/etc/dhcp/dhcpd.conf`       | dhcp configuration (sample file available)                                                                 |
| `/var/www/html`              | default web directory                                                                                      |

## important files Enterprise linux

| File                              | Description                                                                       |
| --------------------------------- | --------------------------------------------------------------------------------- |
| `/var/log/secure`                 | authentication logs                                                               |
| `/var/cache/dnf/`                 | dnf cache                                                                         |
| `/etc/dnf/dnf.conf`               | dnf configuration -> keepcache=1 (will not delete files after succesfull install) |
| `/etc/yum.repos.d/`               | repository configuration -> ending in .repo and in INI format                     |
| `/etc/sysconfig/network-scripts/` | network configuration dir (ifcfg-<interface> commands)                            |
| `/etc/httpd/conf/httpd.conf`      | apache configuration file                                                         |
| `/etc/selinux/config`             | selinux configuration                                                             |
| `/var/log/audit/audit.log`        | selinux logs                                                                      |

## Important files Debian based

| File                        | Description                                                           |
| --------------------------- | --------------------------------------------------------------------- |
| `/var/log/auth.log`         | authentication logs                                                   |
| `/etc/apt/sources.list`     | repository configuration                                              |
| `/etc/apt/sources.list.d/`  | additional repository configuration (mostly for third party software) |
| `/var/cache/apt/archives/`  | apt cache                                                             |
| `/etc/network/interfaces`   | network configuration (ifup and ifdown commands)                      |
| `/etc/netplan/`             | network configuration (netplan) new method of changing interfaces     |
| `/etc/apache2/apache2.conf` | apache configuration file                                             |
