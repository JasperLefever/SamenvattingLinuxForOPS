# File Permissions

## Standard file permissions

Users -> `/etc/passwd`

Groups -> `/etc/group`

A file has a user owner and a group owner.

```bash
# Following commands require root privileges

# Change group owner of a file
chgrp groupname filename

# Change user owner of a file
chown username filename

# Change both user and group owner
chown username:groupname filename
```

list of special files:

| First character | Meaning          | Example       |
| --------------- | ---------------- | ------------- |
| -               | Regular file     |               |
| d               | Directory        |               |
| l               | Symbolic link    |               |
| c               | Character device | Console cable |
| b               | Block device     | Hard drive    |
| p               | Named pipe       |               |
| s               | Socket           |               |

---

Read Write Execute

Order of permissions -> UserOwner-GroupOwner-Other

> When you are the user owner of a file, then the user owner permissions apply to you. The rest of the permissions have no influence on your access to the file.
>
> When you belong to the group that is the group owner of a file, then the group owner permissions apply to you. The rest of the permissions have no influence on your access to the file.
>
> When you are not the user owner of a file and you do not belong to the group owner, then the others permissions apply to you. The rest of the permissions have no influence on your access to the file.

```bash
# Example
student@linux:~/perms$ ls -lh
total 12K
drwxr-xr-x 2 student student 4.0K 2007-02-07 22:26 AllEnter_UserCreateDelete
-rwxrwxrwx 1 student student    0 2007-02-07 22:21 EveryoneFullControl.txt
-r--r----- 1 student student    0 2007-02-07 22:21 OnlyOwnersRead.txt
-rwxrwx--- 1 student student    0 2007-02-07 22:21 OwnersAll_RestNothing.txt
dr-xr-x--- 2 student student 4.0K 2007-02-07 22:25 UserAndGroupEnter
dr-x------ 2 student student 4.0K 2007-02-07 22:25 OnlyUserEnter
```

In order to change permissions with chmod using symbolic notation:

- first specify who the permissions are for: u for the user owner, g for the group owner, o for others, and a for all. a is the default and can be omitted.
- then specify the operation: + to add permissions, - to remove permissions, and = to set permissions.
- finally specify the permission(s): r for read, w for write, and x for execute.
- multiple operations can be combined with a comma (no spaces!)

```bash
# Example
chmod u=rwx,g=rx,o=rw file.txt

# Add write permission for the group owner
chmod g+w file.txt
```

Also possible with octal notation:

- r = 4
- w = 2
- x = 1

```bash
# Example
chmod 764 file.txt

# would be the same as
chmod u=rwx,g=rw,o=r file.txt
```

```bash
# Print permissions in octal notation
stat -c "%a %n" file.txt
```

Use the `umask` command to set the default permissions for newly created files and directories.

```bash
# Examples
umask 022

# would result in
# files: rw-r--r--
# directories: rwxr-xr-x
# because the default permissions are 666 for files and 777 for directories
# if there are 4 numbers the leading 0 can be omitted and is used for setgid and sticky bit
```

In practice, you will only use umask values:

- 0: doesn't change the default permissions
- 2: take away write permission
- 7: take away all permissions

```bash
mkdir -m 755 directory
# -m sets the permissions
cp -p file.txt file2.txt
# -p preserves the permissions
```

## Advanced file permissions

> Sticky bit: only the owner of a file can delete it
>
> can be seen on the "other" permissions as a t (x is also there)
> or as a T (x is not there)

```bash
# Sticky bit
chmod +t directory

chmod 1777 directory
# the first of the 4 numbers is the sticky bit
```

Setgid bit: files created in a directory inherit the group owner of the directory

> How it works: Normally, when a user creates a file or subdirectory, the file is assigned the group of the user creating it. However, if the setgid bit is set on a directory, new files and subdirectories created within the directory inherit the group of the directory, rather than the group of the user.

The setgid bit is displayed at the same location as the x permission for group owner. The setgid bit is represented by an s (meaning x is also there) or a S (when there is no x for the group owner).

```bash
# Setgid bit
chmod g+s directory

chmod 2777 directory
# first number is the setgid bit (2)
```
