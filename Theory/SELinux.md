# SELinux

- `/etc/selinux/config `
- `/var/log/audit/audit.log`

## Modes

- **Enforcing**: SELinux is enabled and actively enforcing policy.
- **Permissive**: SELinux is enabled but only logs policy violations.
- **Disabled**: SELinux is disabled.

**CHANGES WITH CLI ARE NOT PERSISTENT**

```bash
# check the current mode
getenforce

# set the mode
# 0 = pemissive
# 1 = enforcing
sudo setenforce <0|1>

sestatus # check the current status

# edit the configuration file to make the change permanent
sudo nano /etc/selinux/config
```

## File system

> SELinux has a virtual file system that is mounted at `/sys/fs/selinux`

## DAC vs MAC

- **DAC**: Discretionary Access Control
- **MAC**: Mandatory Access Control

> Files are labeled with a security context that includes a user, role, type, and level.

```bash
ls -Z # check the security context of files MAC

```
