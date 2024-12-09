# Package management

## Enterprise linux

### 1. **dnf Command**

- **List Packages**: `dnf list` shows all available packages; `dnf list --available` and `dnf list --installed` list only available or installed packages, respectively.
- **Search Packages**: `dnf search <string>` finds packages by name or description.
- **Package Info**: `dnf info <package>` provides detailed information, including version, architecture, repository, and description.
- **Install Packages**: `dnf install <package>` installs a package with dependencies; add `-y` to skip confirmation prompts.
- **Upgrade Packages**: `dnf upgrade` updates all installed packages to the latest versions in the repository.
- **Find Packages with Specific Files**: `dnf provides <filename>` finds packages containing a specific file.
- **Remove Packages**: `dnf remove <package>` uninstalls a package and unneeded dependencies.
- **Software Groups**: `dnf grouplist` lists groups (sets of packages), and `dnf groupinstall <groupname>` installs a group of related software.

### 2. **rpm Command**

- **List Installed Packages**: `rpm -qa` shows all installed packages.
- **Query Packages**: `rpm -q <package>` checks if a specific package is installed.
- **List Package Files**: `rpm -ql <package>` lists files installed by a package.
- **Install/Upgrade Packages**: `rpm -Uvh <package>.rpm` installs or upgrades an RPM file, but does not handle dependencies.
- **Remove Packages**: `rpm -e <package>` removes a package.

## Debian-based Linux

### `dpkg` Commands:

- **`dpkg -i <package.deb>`**: Installs a `.deb` package but requires manual dependency handling.
- **`dpkg -r <package>`**: Removes a package while keeping configuration files.
- **`dpkg -l`**: Lists all installed packages.
- **`dpkg -l <package>`**: Shows information about a specific package.
- **`dpkg -S <file_path>`**: Finds the package that installed a specific file.
- **`dpkg -L <package>`**: Lists all files and directories installed by a specific package.

### `apt-get` Commands:

- **`apt-get update`**: Updates the local package index with the latest versions from repositories.
- **`apt-get upgrade`**: Updates all installed packages to their latest versions.
- **`apt-get install <package>`**: Installs a package along with its dependencies.
- **`apt-get remove <package>`**: Removes a package without deleting configuration files.
- **`apt-get purge <package>`**: Removes a package along with all configuration files.
- **`apt-get clean`**: Clears out the local repository of downloaded package files.
- **`apt-cache search <term>`**: Searches for packages related to a specific term.

### `apt` Commands (Modern Alternative to `apt-get`):

- **`apt update`**: Syncs package index, similar to `apt-get update`.
- **`apt upgrade`**: Upgrades all installed software, like `apt-get upgrade`.
- **`apt install <package>`**: Installs a package with dependencies.
- **`apt search <string>`**: Searches repositories for packages matching a term.
- **`apt remove <package>`**: Removes a package.
- **`apt purge <package>`**: Completely removes a package and its configuration files.

### Configuration and Repositories:

- **`/etc/apt/sources.list`**: Main configuration file listing sources for repositories.
- **`/etc/apt/sources.list.d/`**: Additional repository sources often used for third-party software.

`apt` is typically favored for interactive use due to its user-friendly design, while `apt-get` is preferred in scripts for its stability.
