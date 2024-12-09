# Users and Groups management

## Users

| Command           | Description                                          | Extra                       |
| ----------------- | ---------------------------------------------------- | --------------------------- |
| `whoami`          | shows current username                               |                             |
| `who`             | shows all users that are logged in (local or remote) |                             |
| `w`               | shows who is logged in and what they are doing       |                             |
| `id`              | shows user and group information                     |                             |
| `su`              | switch user                                          | login als root: `sudo su -` |
| `sudo journalctl` | show logs as root user (systemd systems)             |                             |

### User management -> all require sudo

| Command                              | Description                               | Extra                                                                                                                                                                                      |
| ------------------------------------ | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `useradd`                            | add a user                                | `-m` create home directory <br> `-s` set shell, default is `/bin/bash` <br> `-c` comment <br> `-g` group <br> `-G` additional groups <br> `-u` UID <br> `-d`set different home directory r |
| `userdel -r <user>`                  | delete a user                             |                                                                                                                                                                                            |
| `usermod`                            | modify a user same options as useradd     |                                                                                                                                                                                            |
| `passwd <user>`                      | change/set user password                  |                                                                                                                                                                                            |
| `chage`                              | change password expiration information    |                                                                                                                                                                                            |
| `getent <passwd/group> <user/group>` | get user/group information from databases |                                                                                                                                                                                            |
| `chsh -s <shell>`                    | change shell of current user              |                                                                                                                                                                                            |

### Locking user accounts

| Command                 | Description         | Extra |
| ----------------------- | ------------------- | ----- |
| `passwd -l user`        | lock user account   |       |
| `passwd -u user`        | unlock user account |       |
| `usermod -L user`       | lock user account   |       |
| `usermod -U user`       | unlock user account |       |
| `chsh -s /sbin/nologin` | lock user account   |       |

> also possible to lock accounts by changing the shell to `/sbin/nologin` or `/bin/false`

> Locking will prevent the user from logging on to the system with his password by putting a ! in front of the password in /etc/shadow.

> Disabling with passwd will erase the password from /etc/shadow.

### Alternative user management tools

> The `adduser` and `deluser` commands are wrappers around `useradd` and `userdel` respectively. They are more user friendly and interactive. But not suitable for scripting. Only available on Debian based systems.

## Groups

Here is a summary of the main commands for managing groups in Linux, based on the information provided:

### 1. **Creating and Managing Groups**

- **`groupadd [group_name]`**: Creates a new group with the specified name.

  ```bash
  groupadd tennis
  ```

- **`groupmod -n [new_name] [old_name]`**: Renames an existing group.

  ```bash
  groupmod -n darts snooker
  ```

- **`groupdel [group_name]`**: Deletes the specified group.
  ```bash
  groupdel tennis
  ```

### 2. **Managing Group Membership**

- **`usermod -a -G [group] [user]`**: Adds a user to a group without removing them from other groups (`-a` appends the group).

  ```bash
  usermod -a -G tennis inge
  ```

- **`groups`**: Displays the groups the current user belongs to.
  ```bash
  groups
  ```

### 3. **Delegating Group Administration**

> Group administrators do not have to be a member of the group. They can remove themselves from a group, but this does not influence their ability to add or remove members.

- **`gpasswd -A [admin_user] [group]`**: Assigns group administration privileges to a user for the specified group.

  ```bash
  gpasswd -A serena sports
  ```

- **`gpasswd -a [user] [group]`**: Adds a user to a group as an administrator.

  ```bash
  gpasswd -a harry sports
  ```

- **`gpasswd -d [user] [group]`**: Removes a user from the group.

  ```bash
  gpasswd -d serena sports
  ```

- **`gpasswd -A "" [group]`**: Removes all group administrators from a group.

### 4. **Starting a Child Shell with a New Group**

- **`newgrp [group]`**: Starts a new shell session with the specified group as the temporary primary group.
  ```bash
  newgrp tennis
  ```

### 5. **Editing Group Files Directly**

- **`vigr`**: Opens the `/etc/group` file for manual editing with file locking.

These commands are essential for Linux administrators managing group permissions and memberships efficiently.
