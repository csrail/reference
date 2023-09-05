---
layout: default
title: sys admin 
---
# Cockpit

Readings
- [web console](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/managing_systems_using_the_rhel_9_web_console/getting-started-with-the-rhel-9-web-console_system-management-using-the-rhel-9-web-console#logging-in-to-the-web-console_getting-started-with-the-rhel-9-web-console)

# Shell
Package Management System:
- rpm.
- Manage software across multiple workstations through metadata: install, read, update and delete software; c.f. software installed using tarball which doesn't have metadata information about the package contents, where the package came from etc
- A Linux distribution builds a binary from the source code, gathers the binaries with documentation, config files and scripts, signs the package, deploys it, ready for distribution via FTP or NFS
- The rpm repository is maintained by RedHat, software in this repository is open-source as meets strict testing requirements for distribution.

- yum.
- Builds on top of rpm utilities with a key focus: yum manages dependencies.
- dnf supersedes yum.
- `/etc/yum.conf`, yum config file
- `/etc/yum.repos.d/`, directory of config files specific to a package to find their repo
- `/var/log/dnf.log`, supersedes `/var/log/yum.log`
- `/var/cache/dnf/` - cached package metadata location, supersedes `/var/cache/yum/`
- `/var/lib/rpm/` - after installation, package metadata is stored in a database here; to query this database, use `rpm`.
- Package metadata downloaded manages dependencies, pre-installation scripts, installation procedure and post-installation scripts
- Use `dnf` followed by a __subcommand__ and argument to run operations

Searching for packages:
- `dnf search {KEYWORD}` finds packages related to keyword which can be the name or description of the package
- `dnf provides {KEYWORD}` finds packages related to keyword which can be part of a name, description, command, config file etc. included in the package
- `dnf info {PACKAGE}`
- `dnf list {PACKAGE}`
- `dnf list available`
- `dnf list installed`
- `dnf list all`
- `dnf deplist {PACKAGE}` finds dependencies of a package

Installing packages:
- `dnf install {PACKAGE}`
- `dnf reinstall {PACKAGE}` for repairing an installation

Removing packages:
- `dnf erase {PACKAGE}` removes only the selected package and ignores dependencies
- `dnf remove {PACKAGE}` removes the package and its dependencies
- `dnf history undo {ID}` undo an entire transaction based on history ID
  - `dnf history`
  - `dnf history info {ID}`

Updating packages:
- `dnf check-update` checks if new updates are available
- `dnf update {PACKAGE}` updates a single package
- `dnf update` updates all packages

Group packages:
- Groups of packages are bundled together to provide an environment / suite of tools e.g. postgresql, scientific support
- `dnf grouplist`
- `dnf groupinfo {PACKAGE_GROUP}`
- `dnf groupinstall {PACKAGE_GROUP}`
- `dnf groupremove {PACKAGE_GROUP}`

- `rpm -qi firefox`



# Shell
- `# cat /var/log/messages | less`
- `# dmesg | less`
- `# journalctl`

One of the many entries in `journalctl` is the logging of users running commands on a terminal, including the timestamp, what privilege and in which directory.

Readings
- [systemd](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/introduction-to-systemd_configuring-basic-system-settings)
- [log files, syslog](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/assembly_troubleshooting-problems-using-log-files_configuring-basic-system-settings)

# Shell
Commands following the `$` symbol is a shell prompt for a normal user.  Commands following the `#` symbol are run by a superuser.

- Run `$ systemctl list-units --type service` for a listing of active services.
- Run `$ systemctl status firewalld.service` to check the state of the firewall.
- Run `# systemctl enable --now firewalld` to start the firewall.  `enable` indicates the service `firewalld` should be started automatically after a reboot.  `--now` flag is equivalent to `# systemctl start firewalld`
- The full operational name for `firewalld` is `firewalld.service`.
- `# systemctl start name.service`, `# systemctl stop name.service`, `# systemctl restart name.service`
- `# systemctl reload name.service`, reload configuration without stopping execution of the service
- `# systemctl enable name.service` and `# systemctl disable name.service`

Readings
- [firewalld, systemctl](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/managing-system-services-with-systemctl_configuring-basic-system-settings)

# Shell
Shell environment variables are declared with the following syntax: `KEYWORD="value"`, the shell variable uses capital letters, with no spaces between the equal operator and its surrounding operands.  To fetch the value of variable, use the dollar sign: `$KEYWORD`.

Set up personal scripts to group tasks together.
- In the `~/.bashrc` file, include your home directory script files in the shell execute path: `export PATH="$HOME/scripts/bin:$PATH`.
- Create a new file `ide.sh`, placing it in the directory `~/scripts/bin`.
- Make the file executable via `chmod +x ide.sh` and including the shebang declaration at the top of the file: `#!/bin/bash`.
- After commands are included in the `~/scripts/bin/ide.sh` file, the command can be run by typing `ide.sh` in the command line
- All commands executed as part of the script occur in a spawned subshell.
- The spawned subshell resolves all the actions before returning control to the parent subshell.  This means that any context actions which place the shell in different directories are not carried over to the parent shell on control being returned.
- An opportunity to operate within the subshell can occur if the subshell creates a listener process like a server.  Running `Ctrl+C` to cancel the server will instead cancel the subshell thereby killing any processes that the subshell has created.
- The scripts must create processes that move standard output and standard errors into `/dev/null` otherwise the parent shell will log the information; and also run the process in the background.
- Place `\` at the end of a line to continue the statement to the next line.

```bash
#!/bin/bash

reset='\033[0m'
green='\033[1;32m'
echo -e "$green ide setup, executed$reset"
 
sh /opt/pycharm-2023.2/bin/pycharm.sh &>/dev/null &

firefox --new-tab -url "http://127.0.0.1:4000/" \
        --new-tab -url "$HOME/PycharmProjects/theodinproject2023/restaurant/dist/index.html" \
        &>/dev/null &
```

# Shell
- `info coreutils` to open documentation on GNU utilities; more comprehensive than `man` for the topics discussed
- `tldr [COMMAND]` outputs quick reference documentation on a command; useful for getting an initial understanding
- `tldr --update` to update the cache; updated to improve documentation so fetching as needed
- `sh /opt/pycharm-2023.2/bin/pycharm/.sh &>/dev/null &` start the pycharm binary, redirects standard output and standard error into `/dev/null` and moves the application into the background to resume terminal usage
- `man bash /REDIRECTION` and `man bash /Redirecting Standard Output and Standard Error` to learn more about `&>`

# Shell
- `~/.bashrc` file
- Set up aliases:

```bash
CURRENT_PROJECT="$HOME/PycharmProjects/theodinproject2023/restaurant/"

alias ll="ls -al --block-size=kB"
alias rm="rm -i"
alias current="cd $CURRENT_PROJECT"
```

- Many sys_admin commands happen in startup and shutdown scripts within `etc/rc.d`.  Caution here.

Host Information:
- `uname -s` shows operating system type.
- `uname -m` shows the architecture, equivalent to `arch`.
- `uname -a` for a verbose description.

Job Control:
- `top -b` lists current processes using up CPU, outputs in text format to be parsed or accessed by a script.
- `ps aux` lists current processes by PID, output can be piped to `grep` or `sed`
- `pstree` lists current processes in a tree format, very useful for understanding how the processes were spawned; `-p` option shows PIDs and process names, more verbose.
- `cron` is the root version of `at`; the daemon executing scheduled entries on`/etc/crontab`.  See `man 4 crontabs`

Readings:
- [the linux documentation project: advanced bash scripting](https://tldp.org/LDP/abs/abs-guide.pdf)
- [abs: commands](https://tldp.org/LDP/abs/html/part4.html)
- [abs: sys_admin commands](https://tldp.org/LDP/abs/html/system.html)
- [iptables](https://www.frozentux.net/iptables-tutorial/iptables-tutorial.html)

# Keyboard Input

- Settings > Keyboard > Input Sources > + > Greek (polytonic).
- `Super Key + Spacebar` to switch between inputs.