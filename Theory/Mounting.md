# Mounting and Unmounting

## Mounting

> Mounting is the process of making a filesystem available to the operating system. This is done by attaching the filesystem to a directory in the filesystem tree. The directory to which the filesystem is attached is called the mount point. The mount point is the location in the filesystem where the mounted filesystem will be accessible.

### Mounting a filesystem

> To mount a filesystem, you need to know the device name and the mount point. The device name is the name of the device file that represents the filesystem. The mount point is the directory in the filesystem tree where the filesystem will be attached.

```bash
# Make a directory to use as the mount point
sudo mkdir /mnt/data

# make sure a filesystem is present on the device
# mount the filesystem
sudo mount /dev/sdb1 /mnt/data # option -t can be used to specify the filesystem type eg. -t ext4
```

## available file systems

- `/etc/filesystems` contains a list of filesystems that the kernel can support

Mount reads the whole file to determine the filesystem type. If it does not exist or ends with a `*` it will read the filesystems from `/proc/filesystems`

### Unmounting a filesystem

> To unmount a filesystem, you need to know the mount point. The mount point is the directory in the filesystem tree where the filesystem is attached.

```bash
sudo umount /mnt/data
```

### Viewing mounted filesystems

```bash
findmnt

## Handy options
findmnt --real # shows the real mount point
findmnt --fstab # shows the mount point as defined in /etc/fstab
findmnt --json # shows the output in json format
findmnt --pairs # shows the output in key value pairs
findmnt --df # shows the output in the same format as df

# or

mount # deprecated

# or

df -h # diskfree    -h human readable

# or

cat /proc/mounts

# or

lsblk

# or

cat /etc/mtab

```

### Check available space

```bash
df -h

# or

du -sh /mnt/data # this means du -sh on a mount point gives the total amount used by the file system in that partition.
```

### Mounting at boot

> To mount a filesystem at boot, you need to add an entry to the `/etc/fstab` file. The `/etc/fstab` file contains a list of filesystems that should be mounted at boot.

```bash
# /etc/fstab
/dev/sdb1 /mnt/data ext4 defaults 0 0
```

> this also results in the mount command being able to mount the filesystem without specifying the partition

```bash
sudo mount /mnt/data
```

### Mount options

```bash
# /etc/fstab
/dev/sdb1 /mnt/data ext4 defaults 0 0

# options
# defaults: use the default options
# ro : mount the filesystem read-only
# rw : mount the filesystem read-write
# noexec : do not allow execution of binaries on the filesystem
# nosuid : do not allow set-user-identifier or set-group-identifier bits to take effect
# noacl : do not allow to change the acl
```

```bash
sudo mount -o remount,ro /mnt/data # remount the filesystem read-only
```

### Mounting a remote filesystem

```bash
# for samba
# make sure cifs-utils is installed
sudo mount -t cifs //server/share /mnt/data -o username=user,password=pass
```

```bash
# with nfs
sudo mount -t nfs server:/share /mnt/data
```

### using uuid

```bash
# get the uuid
sudo blkid /dev/sdb1
#or
tune2fs -l /dev/sdb1 | grep UUID

# /etc/fstab
UUID=12345678-1234-1234-1234-123456789012 /mnt/data ext4 defaults 0 0
```
